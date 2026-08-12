# Getting started

After installing, these are the initial tweaks for a HERD server. Everything here
can be done from the terminal (SSH/console) or from [Cockpit](cockpit.md).

## Access

- **SSH (by key):** `ssh user@<ip>` — remember SSH is **key-only** (see
  [Install via ISO › Remote access](installation-iso.md#remote-access)).
- **Console:** physical/IPMI, with the admin user or root.
- **Cockpit:** `https://<ip>:9090` (user + password).

## Elevate privileges (sudo)

The administrator user (`wheel` group) uses `sudo`:

```bash
sudo dnf upgrade --refresh
```

## Update the system

First things first — bring the system up to date:

```bash
sudo dnf upgrade --refresh
sudo systemctl reboot   # if a new kernel was installed
```

## Hostname

```bash
sudo hostnamectl set-hostname herd-01.example.com
```

## Network

The network comes up via **DHCP**. For a static IP, use NetworkManager
(`nmcli`):

```bash
# list connections and the interface
nmcli connection show
nmcli device status

# set a static IP (adjust connection name and values)
sudo nmcli connection modify "System eth0" \
  ipv4.method manual \
  ipv4.addresses 192.168.0.10/24 \
  ipv4.gateway 192.168.0.1 \
  ipv4.dns "1.1.1.1 8.8.8.8"

sudo nmcli connection up "System eth0"
```

!!! tip "Via Cockpit"
    You can do the same under **Networking** in [Cockpit](cockpit.md), with a
    graphical interface.

## Timezone and time

```bash
timedatectl                              # current state
sudo timedatectl set-timezone America/Sao_Paulo
```

Time synchronization (`chronyd`) is already active.

## Locale / language

The system ships `pt_BR.UTF-8` and `en_US.UTF-8`. To change the default:

```bash
localectl                                # current state
sudo localectl set-locale LANG=en_US.UTF-8
```

## Users and SSH keys

Create another administrator:

```bash
sudo useradd -m -G wheel mary
sudo passwd mary
# add their public key:
sudo install -d -m 700 /home/mary/.ssh
echo "ssh-ed25519 AAAA... mary@pc" | sudo tee -a /home/mary/.ssh/authorized_keys
sudo chown -R mary:mary /home/mary/.ssh
sudo chmod 600 /home/mary/.ssh/authorized_keys
```

You can also manage accounts under **Accounts** in [Cockpit](cockpit.md).

## Open ports for your applications

`firewalld` only allows **SSH** and **Cockpit** by default. For a web service,
for example:

```bash
sudo firewall-cmd --permanent --add-service=http --add-service=https
sudo firewall-cmd --reload
sudo firewall-cmd --list-all
```

See more in [Administration › Firewall](administration.md#firewall).

## Next steps

- [Web console (Cockpit)](cockpit.md)
- [Security and compliance](security.md) — run `herd-compliance-scan`.
- [Administration](administration.md) — services, storage, logs, updates.
