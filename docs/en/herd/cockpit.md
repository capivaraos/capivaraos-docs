# Web console (Cockpit)

Herd ships with **Cockpit**, a browser-based administration panel. You can view
system health, manage services, network, storage, accounts, read logs and even
open a terminal — without installing anything extra.

## Access

Open in your browser:

```
https://<server-ip>:9090
```

!!! note "Self-signed certificate"
    On first access the browser warns about the certificate (self-signed). That's
    expected on a freshly installed server — proceed. For production, you can
    install your own certificate in `/etc/cockpit/ws-certs.d/`.

The service is activated **on demand** (`cockpit.socket`): it only starts when
someone accesses port 9090, which is already allowed in the firewall.

## Login

- Use a **system user** with its **password** (the same login as the console).
- **Root is blocked** in Cockpit by default — use the administrator user
  (`wheel` group) you created.

!!! tip "Administrative access"
    After logging in, click **"Turn on administrative access"** (top corner) and
    enter the password again. This unlocks actions that require privilege
    (installing updates, managing services, storage, etc.).

## What you can do

| Module | For what |
|---|---|
| **Overview** | Health (CPU, memory), system identity, time, hostname |
| **Logs** | Browse the journal (filter by priority, service, period) |
| **Storage** | Disks, partitions, LVM, filesystems, mounts |
| **Networking** | Interfaces, IP, firewall, bonds/bridges/VLANs |
| **Accounts** | Create/edit users, groups, SSH keys, password expiration |
| **Services** | Enable/stop/restart systemd units, view status |
| **Software updates** | Apply security and system updates |
| **Terminal** | The server shell right in the browser |

## CapivaraOS identity

Herd's Cockpit ships with the CapivaraOS branding (login screen and header) and
**without** documentation links pointing to third parties — the docs link leads
to this documentation.

## Security tip

Cockpit is a powerful administration door. On internet-facing servers, consider:

- Accessing it over **VPN** or an **SSH tunnel** instead of exposing 9090
  publicly:
  ```bash
  ssh -L 9090:localhost:9090 user@<ip>
  # then open https://localhost:9090 on your PC
  ```
- Restricting the source in `firewalld` (allow 9090 only from the administration
  network).

See [Security and compliance](security.md).

---

*Content produced with AI assistance, under continuous review. Found an error? [Open an issue](https://github.com/capivaraos/capivaraos-herd/issues).*
