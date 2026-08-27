# Requirements

Herd Community 1.0.0 ships for **x86_64** (64-bit). **aarch64** (ARM) support
arrives in version 1.1.

## Minimum and recommended hardware

| Component | Minimum | Recommended |
|---|---|---|
| **Architecture** | x86_64 (64-bit) | x86_64 |
| **CPU** | 1 core | 2+ cores |
| **RAM** | 1 GB | 2 GB or more |
| **Disk** | 10 GB | 20 GB or more |
| **Network** | 1 Ethernet interface | — |
| **Firmware** | Legacy BIOS **or** UEFI | UEFI |

!!! note "Real server vs. testing"
    The minimums above are enough to boot and administer the system. For
    production workloads (databases, containers, etc.), size CPU/RAM/disk
    according to your application.

## Firmware and boot

- The installer ISO works on both **UEFI** and **legacy BIOS**.
- On UEFI with **Secure Boot**, Herd uses Fedora's signed `shim` — boot works
  without disabling Secure Boot on most hardware.

## Virtualization and cloud

Herd runs well on virtualization and cloud. The **qcow2** image already ships
`qemu-guest-agent` and `cloud-init`:

| Platform | Note |
|---|---|
| **KVM/QEMU, libvirt** | Use the `qcow2` image directly |
| **Proxmox VE** | Import the `qcow2` as the VM disk |
| **VMware / VirtualBox** | Use the installer ISO, or convert the `qcow2` |
| **Cloud (OpenStack, etc.)** | `qcow2` + `cloud-init` (NoCloud/ConfigDrive datasource) |

!!! tip "Firewall is restrictive by default"
    By default `firewalld` allows only **SSH (22)** and **Cockpit (9090)**. Plan
    to open the ports your applications need — see
    [Administration › Firewall](administration.md#firewall).

## Network

- By default the network is configured via **DHCP**.
- A static IP can be set during installation (Anaconda), later via
  [`nmcli`](getting-started.md#network) or through [Cockpit](cockpit.md).
- IPv6 via DHCPv6 is supported (the `dhcpv6-client` service is intentionally
  left allowed in the firewall).

---

*Content produced with AI assistance, under continuous review. Found an error? [Open an issue](https://github.com/capivaraos/capivaraos-herd/issues).*
