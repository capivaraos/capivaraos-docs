# Install via ISO (bare-metal)

This guide covers installing HERD Community on a **physical** server or a **VM**
using the installer ISO. The installer is **Anaconda in text mode** — ideal for
_headless_ servers, with no dependency on a graphical interface.

!!! info "Offline installation"
    The ISO is **self-contained**: packages are embedded, so the installation
    **does not need internet access**.

## 1. Prepare the boot media

Write the ISO to a USB drive (≥ 2 GB) or mount it as virtual media (iDRAC/iLO/IPMI,
or the VM's "CD/DVD").

=== "Linux/macOS (dd)"

    ```bash
    lsblk   # find the USB device (e.g. /dev/sdX)
    sudo dd if=CapivaraOS-HERD-1.0.0-x86_64.installer.iso of=/dev/sdX bs=4M status=progress oflag=sync
    ```

    !!! warning "Confirm the device"
        `dd` on the wrong disk erases data irreversibly. Check with `lsblk`
        first.

=== "Windows"

    Use **Balena Etcher** or **Rufus** (DD/image mode).

=== "VM"

    Point the VM's CD/DVD drive to the `.iso` file. No need to burn a USB drive.

## 2. Boot from the ISO

Enter the server's boot menu (usually **F11/F12/Del/Esc**) and select the media.
The CapivaraOS GRUB menu appears — choose **Install CapivaraOS HERD**.

Anaconda starts in **text mode** and shows a numbered menu. Items marked `[x]`
are already configured; those marked `[ ]` need your attention.

## 3. Configure the required items

The profile comes ready (pt_BR + en_US locale, ABNT2 keyboard,
America/Sao_Paulo timezone, DHCP network, offline installation source). You need
to set:

### Disk (installation destination)

Choose the number for **"Installation Destination"**, select the disk and confirm
automatic partitioning (or define your own).

!!! tip "Partitioning"
    Automatic uses LVM and works for most cases. For specific layouts (e.g.
    separate `/var`, quota sizes), configure manually.

### Root password

Set it under **"Root password"**. Direct root login over SSH is **disabled** by
default (see [Security](security.md)); the root password is for the **console**
(physical/IPMI) and recovery.

### Create an administrator user

This step matters — it is the account you'll use day to day and in
[Cockpit](cockpit.md) (Cockpit **blocks root login**).

1. Choose the number for **"User creation"**.
2. Type `1` to **create the user** — the form expands.
3. Fill in the **user name** and **password**.
4. Mark **Administrator** (`[x]`) — this puts the account in the `wheel` group
   (allows `sudo` and enabling administrative access in Cockpit).
5. Type `c` to continue.

## 4. Begin the installation

Back on the main menu, with all items `[x]`, type `b` (_begin_) to install. When
it finishes, **remove the media** and reboot.

## 5. First boot

In the GRUB menu you'll see the **CapivaraOS HERD** entry. The system boots to
text mode and prints, on the console, the Cockpit address:

```
CapivaraOS HERD Community 1.0.0  ·  headless server (Fedora 44)
Web console: https://<ip>:9090/
```

## <a id="remote-access"></a>6. Remote access — read this

By default SSH accepts **keys only** (`PasswordAuthentication no`). That means
right after installing **you cannot log in over SSH with a password**. Options:

- **Console first (recommended):** log in at the physical/IPMI console with the
  admin user and add your public key:
  ```bash
  install -d -m 700 ~/.ssh
  echo "ssh-ed25519 AAAA... you@machine" >> ~/.ssh/authorized_keys
  chmod 600 ~/.ssh/authorized_keys
  ```
  Then: `ssh user@<ip>`.
- **Cockpit:** the web console at `https://<ip>:9090` accepts **user + password**
  (it is not SSH) — you can administer without a key. See [Web console](cockpit.md).
- **Really need SSH by password?** You can relax the policy, with security
  caveats — see [Security › Adjusting SSH](security.md#ssh).

## Next steps

- [Getting started](getting-started.md) — network, hostname, updates.
- [Web console (Cockpit)](cockpit.md) — administer from the browser.
- [Security and compliance](security.md) — what's hardened.

---

*Content produced with AI assistance, under continuous review. Found an error? [Open an issue](https://github.com/capivaraos/capivaraos-herd/issues).*
