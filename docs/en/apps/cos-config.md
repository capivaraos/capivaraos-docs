# COS Config Center

**COS Config Center** handles, in a graphical interface, nine things that on Linux normally take the command line. It is exclusive to CapivaraOS and ships preinstalled on all three desktop editions.

To open it, look for **COS Config Center** in the application menu.

It asks for the administrator password only for settings that touch the system, and always warns first.

## Software and drivers

### Codecs and extra repositories

Enables the **RPM Fusion** repositories and **Flathub**, and installs the codecs Fedora cannot ship by default for patent reasons. This is what makes video and music "just play".

!!! note "These are third-party repositories"
    RPM Fusion and Flathub are run by their own communities, not by CapivaraOS or Fedora. Some formats are covered by patents in certain countries — worth checking what applies where you live.

## Disks and maintenance

### Free disk space

Shows what can safely be removed, split into two lists: what lives in **your user account** (trash, thumbnails, unused Flatpak runtimes, container images) and what lives in the **system** and needs the password (already-installed packages, old logs, crash dumps).

Each item shows how much it takes before you decide. Nothing is removed unless you tick it.

### Disks at startup

Mounts your extra disks and partitions automatically when the computer starts, without you editing `/etc/fstab` by hand.

### System log size

Caps how much space system logs may use, so they do not grow forever and fill the disk.

### System check

A quick health check: disk space, running services, the video driver in use and errors since the machine booted.

At the end it builds a **report to paste on the forum** when you ask for help. The report does **not** include your name, the computer's name or network addresses.

## Keyboard and language

### Brazilian keyboard

Choose between **ABNT2**, **US International** and **US** — and, on US International, enable typing **ç** with `'` followed by `c`, the detail that most annoys anyone writing Portuguese on a US keyboard.

The same layout can be applied to the login screen as well.

### Language and formats

Separates two things that usually come glued together: the **language** of menus and applications, and the **formats** for date, number and currency. You can run the system in English while keeping Brazilian date and currency.

It applies system-wide, login screen included, and takes effect after you log out and back in.

## Network and security

### Firewall

Shows what is open to the network in plain words — "share folders on the network", "remote terminal access", "web server" — instead of port numbers.

There is also **strict mode**, which closes ports above 1024 and lets through only what you turned on.

### Remote access and network

Changes the **computer name** on the network, turns **SSH remote access** on or off, creates your **SSH key** and configures **encrypted DNS**.

!!! warning "Two caveats in this release"
    SSH password login stays enabled — we do not alter `sshd_config`. And encrypted DNS can break names that only exist on your local network; if something stops resolving after you enable it, switch back to "Automatic".

## About this release

COS Config Center ships with Series 2 as **0.11.x**, a first public release with these nine modules. 1.0.0 will come after feedback from the people using it.

**Not in this release:** the NVIDIA driver (that is [COS Dev Center](cos-dev.md)'s job), dual boot configuration and video acceleration.

**System updates** do not go through here either — those belong to the software centre or `dnf`. See [Updating the system](../update/index.md).
