# Cloud / VM deployment (qcow2)

The **qcow2** image already ships `cloud-init` and `qemu-guest-agent`, which makes
it ideal for spinning up quickly on cloud, KVM/libvirt, Proxmox and the like.
`cloud-init` handles the **user**, **SSH key**, **hostname** and **network** on
first boot.

!!! info "Image = template"
    The qcow2 is a _template_. Each VM created from it is personalized by
    `cloud-init` on first boot (from the datasource).

## Option A — KVM/libvirt with a NoCloud seed

Without a cloud provider, the simplest way to feed `cloud-init` is a **NoCloud**
datasource: a small ISO with two files, `meta-data` and `user-data`.

### 1. Create the seed files

`meta-data`:

```yaml
instance-id: herd-01
local-hostname: herd-01
```

`user-data` (note the `#cloud-config` on the first line):

```yaml
#cloud-config
users:
  - name: admin
    groups: [wheel]
    sudo: "ALL=(ALL) NOPASSWD:ALL"
    shell: /bin/bash
    ssh_authorized_keys:
      - ssh-ed25519 AAAA... you@machine
# optional password — used for CONSOLE and Cockpit (SSH is key-only)
chpasswd:
  expire: false
  users:
    - name: admin
      password: "change-this-password"
      type: text
```

!!! warning "SSH is key-only"
    HERD ships with `PasswordAuthentication no`. **Include your public key** in
    `ssh_authorized_keys`, otherwise you won't get in over SSH. The password (if
    set) is for the console and for [Cockpit](cockpit.md).

!!! note "Passwordless sudo"
    The example uses `NOPASSWD:ALL` (sudo without a password prompt) — handy in
    cloud with key-only login. For tighter control, switch to `ALL=(ALL) ALL`
    and set a user password; then `sudo` will require it.

### 2. Build the seed ISO

```bash
genisoimage -output seed.iso -volid cidata -joliet -rock user-data meta-data
# (or: cloud-localds seed.iso user-data meta-data)
```

### 3. Launch the VM

```bash
cp CapivaraOS-HERD-1.0.0-x86_64.qcow2 herd-01.qcow2

virt-install --name herd-01 --memory 2048 --vcpus 2 \
  --disk path=herd-01.qcow2,format=qcow2,bus=virtio \
  --disk path=seed.iso,device=cdrom \
  --os-variant fedora-unknown --import \
  --network network=default,model=virtio --graphics none
```

Find the IP (`virsh domifaddr herd-01`) and connect: `ssh admin@<ip>`.

## Option B — Cloud provider

1. **Upload the qcow2** as an image/template in your provider (OpenStack Glance,
   etc.) — or import it as a disk (Proxmox: `qm importdisk`).
2. Create the instance from the image.
3. Provide the **user-data** (same `#cloud-config` as above) through the
   provider's mechanism (metadata service / ConfigDrive). `cloud-init` applies it
   on first boot.

!!! tip "qemu-guest-agent"
    Already installed and enabled — the virtualization host can query the IP,
    quiesce snapshots and shut down gracefully.

## After it's up

- [Getting started](getting-started.md) — updates, network, timezone.
- [Web console (Cockpit)](cockpit.md) — `https://<ip>:9090`.
- [Security and compliance](security.md).

---

*Content produced with AI assistance, under continuous review. Found an error? [Open an issue](https://github.com/capivaraos/capivaraos-herd/issues).*
