# Changelog

Key milestones in the development of CapivaraOS's distros.

> This changelog only covers CapivaraOS's branding and system tweaks — it doesn't include the upstream Fedora, KDE, Xfce or GNOME package history.

---

## CapivaraOS Marsh

### 1.2.4
*August 2026*

- Fix: CapivaraOS Marsh now installs on computers with UEFI firmware. Previously the installation went almost all the way and then failed at the bootloader step, because the installer did not recognize CapivaraOS as its own profile and looked for the boot files in the wrong place. `(BUG-38)`
- Fix: the installed system now boots on a wider range of hardware (for example, netbooks with eMMC storage). Previously, on machines whose storage differed from the one used to build the image, the system could fail to find the disk on first boot and drop into emergency mode. `(BUG-40)`

### 1.2.3
*August 2026*

- Minor licensing fixes to the photo wallpapers.

### 1.2.2
*July 2026*

- New visual identity: the walking capybara logo across all branding — wallpapers, icons, boot splash, login screen and installer. `(DOC-6)`
- The dock's application launcher icon is now the capybara head, replacing the KDE logo. `(DOC-6)`
- Fix: the logo no longer overlaps the text on the live session's welcome screen.
- Fix: the ISO now ships with Fedora updates included, avoiding roughly 1,000 update packages on first boot.
- Fix: the logo and photo credit on the photographic wallpapers are no longer cropped off on 4:3 screens.
- Fix: the logo artwork no longer shows up as an option in the wallpaper picker.

### 1.1.2
*June 2026*

- Plymouth now works correctly — boot animation with the CapivaraOS logo displays properly (fix: missing `plymouth-plugin-script` package).
- GRUB title stays correct — boot menu permanently shows "CapivaraOS Marsh", even after system updates.
- Offline update screen — Plymouth shows progress percentage during boot-time updates.
- Fixed a brief duplicate bottom dock appearing right after login.
- Plasma's welcome screen no longer opens on first login.

### 1.1.0
*June 2026*

- First release ported to Fedora 44, from the original Debian/trixie base.

---

## CapivaraOS Pup

### 1.1.8
*August 2026*

- Fix: CapivaraOS Pup now installs on computers with UEFI firmware. Previously the installation went almost all the way and then failed at the bootloader step, because the installer did not recognize CapivaraOS as its own profile and looked for the boot files in the wrong place. `(BUG-38)`
- Fix: the installed system now boots on a wider range of hardware (for example, netbooks with eMMC storage). Previously, on machines whose storage differed from the one used to build the image, the system could fail to find the disk on first boot and drop into emergency mode. `(BUG-40)`

### 1.1.7
*August 2026*

- Fix: the CapivaraOS wallpaper is now applied automatically on the first login (live and installed). Previously you had to change it manually once for it to stick.

### 1.1.2
*July 2026*

- First public beta release of Pup, available to download.
- New visual identity: the walking capybara logo across all branding — wallpapers, icons, boot splash, login screen and installer. `(DOC-6)`
- Fix: the ISO now ships with Fedora updates included. The updates repository was being silently ignored because it used a name reserved by Anaconda, so images shipped with plain Fedora 44.
- Fix: the logo and photo credit on the photographic wallpapers are no longer cropped off on 4:3 screens.
- Fix: the system identification in `os-release` and GRUB now survives future Fedora updates.

### 1.0.0
*June 2026*

- First release of the Xfce spin (Fedora 44), designed for computers with at least 4 GB of RAM.
- Exclusive wallpapers: 13 solid-color backgrounds with the CapivaraOS logo, plus capybara photos from Wikimedia Commons (CC BY/CC BY-SA) with embedded credits.
- Full branding in the graphical installer — shows CapivaraOS logo and colors instead of Fedora's.
- Custom Plymouth boot theme on startup and shutdown.
- LightDM login screen with CapivaraOS visual identity.
- System correctly identified as CapivaraOS Pup in `os-release` and GRUB.

---

## CapivaraOS Snout

### 1.1.15
*August 2026*

- Fix: the login screen shows up correctly again. Previously, when automatic login was turned off, the system did not show the login screen and instead entered an empty session — with no user list and no password field —, and in that state the Terminal could not be opened. Cause: a missing line in the GNOME login screen configuration, introduced when customizing the login wallpaper. `(BUG-42)`
- The Terminal now comes pinned to the app dock for easier access.

### 1.1.9
*August 2026*

- Fix: CapivaraOS Snout now installs on computers with UEFI firmware. Previously the installation went almost all the way and then failed at the bootloader step, because the installer did not recognize CapivaraOS as its own profile and looked for the boot files in the wrong place. `(BUG-38)`
- Fix: the installed system now boots on a wider range of hardware (for example, netbooks with eMMC storage). Previously, on machines whose storage differed from the one used to build the image, the system could fail to find the disk on first boot and drop into emergency mode. `(BUG-40)`

### 1.1.6
*August 2026*

- Minor licensing fixes to the photo wallpapers.

### 1.1.5
*July 2026*

- First public beta release of Snout, available to download.
- New visual identity: the walking capybara logo across all branding — wallpapers, icons, boot splash, GDM login screen and installer. `(DOC-6)`
- Fix: the ISO now ships with Fedora updates included. The updates repository was being silently ignored because it used a name reserved by Anaconda.
- Fix: the logo in GNOME's "About" panel is no longer cropped at the edges.
- Fix: the system identification and the CapivaraOS logo now survive future Fedora updates — three mechanisms meant to reapply them were never actually running.

### 1.0.0
*June 2026*

- First release of the GNOME spin, with its own branding package (`capivaraos-branding`), wallpapers, boot/login theme, and GDM background via dconf.
- Stock Fedora Workstation GNOME environment, with no dock, global menu, or third-party themes.

---

## Herd by CapivaraOS

### 1.0.1
*August 2026*

- One-command hardening: new `herd-harden`, which applies SCAP Security Guide profiles (OSPP, CIS, PCI-DSS) via Ansible, with a dry-run preview by default. `(FEAT-97)`
- Compliance report with evidence: `herd-compliance-scan` now accepts profile aliases and also produces an ARF file (auditing evidence), alongside HTML/XML. `(FEAT-97)`
- FIPS mode documented for Fedora 44 (via `fips=1` on the kernel) and disk encryption (LUKS) as opt-in options.
- The line is now branded **Herd by CapivaraOS**.

### 1.0.0
*August 2026*

- First stable release of the server line: headless Fedora 44 base, SELinux enforcing, hardened SSH, restrictive firewall, auditing on, and the Cockpit web console included. `(PROD-4)`
- qcow2 images (cloud/cloud-init) and a branded installer ISO (x86_64). `(PROD-4)`
