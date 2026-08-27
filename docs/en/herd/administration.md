# Administration

Day-to-day tasks for a Herd server, from the terminal. Almost everything is also
available in [Cockpit](cockpit.md), graphically.

## Updates

Herd uses **dnf** (Fedora base):

```bash
sudo dnf upgrade --refresh          # everything
sudo dnf upgrade --security         # security fixes only
sudo dnf check-update               # see what's new
```

Reboot after a kernel update: `sudo systemctl reboot`.

## Services (systemd)

```bash
systemctl status sshd               # a service's state
sudo systemctl restart sshd         # restart
sudo systemctl enable --now nginx   # enable at boot and start now
systemctl list-units --type=service --state=running
```

A service's logs: `journalctl -u sshd`.

## Logs (journal)

```bash
journalctl -b                       # current boot
journalctl -p err -b                # errors only
journalctl -u <service> -f          # follow in real time
journalctl --since "today"
```

## Storage

```bash
lsblk                               # disks and partitions
df -h                               # filesystem usage
sudo lvs; sudo vgs; sudo pvs        # LVM (the default install uses LVM)
```

To add/expand disks, LVM and mounts, the **Storage** module in
[Cockpit](cockpit.md) is the simplest path.

## Network

Managed by NetworkManager (`nmcli`) — see examples in
[Getting started › Network](getting-started.md#network).

```bash
nmcli device status
nmcli connection show
ip a                                # current addresses
```

## <a id="firewall"></a>Firewall (firewalld)

By default the `public` zone allows **ssh**, **cockpit** and **dhcpv6-client**.

```bash
sudo firewall-cmd --list-all                                   # zone state
sudo firewall-cmd --permanent --add-service=https              # allow a service
sudo firewall-cmd --permanent --add-port=8080/tcp             # allow a port
sudo firewall-cmd --reload                                     # apply
```

Allow a port only from an administration network (safer than opening to
everyone):

```bash
sudo firewall-cmd --permanent --zone=internal --add-source=192.168.10.0/24
sudo firewall-cmd --permanent --zone=internal --add-service=cockpit
sudo firewall-cmd --reload
```

## SELinux

Ships in `enforcing` (recommended to keep):

```bash
getenforce
sudo ausearch -m avc -ts recent     # recent denials
sudo sealert -a /var/log/audit/audit.log   # analysis (if setroubleshoot installed)
```

!!! warning "Don't disable SELinux"
    If a service is blocked, prefer fixing the correct context/boolean rather
    than disabling SELinux. Disabling it removes an important containment layer.

## Shut down / reboot

```bash
sudo systemctl reboot
sudo systemctl poweroff
```

## Next steps

- [Security and compliance](security.md)
- [Support](support.md)

---

*Content produced with AI assistance, under continuous review. Found an error? [Open an issue](https://github.com/capivaraos/capivaraos-herd/issues).*
