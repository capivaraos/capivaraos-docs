# Frequently asked questions

## Is CapivaraOS free?

Yes, completely. It's an open source project — free to download, install and use.

## What's the difference between Marsh, Pup and Snout?

The difference is the desktop environment:

- **Marsh** uses KDE Plasma — full-featured and highly customizable
- **Pup** uses Xfce — lightweight, great on modest hardware
- **Snout** uses GNOME — clean, no layout tweaks

The base system, the visual identity, the two exclusive applications ([COS Dev Center](apps/cos-dev.md) and [COS Config Center](apps/cos-config.md)) and the development toolkit are the same across all three.

## Does CapivaraOS replace Fedora?

Not exactly. CapivaraOS is built on top of Fedora — it inherits Fedora's package system, release cycle and hardware compatibility, but adds a visual identity of its own, two exclusive applications and a development environment ready at first boot.

## Can I install Fedora packages on CapivaraOS?

Yes. Since the base is Fedora, all Fedora repositories and packages work normally. DNF and Flatpak are both available.

## Does CapivaraOS support Flatpak?

Yes, Flatpak comes pre-configured. You can install apps from Flathub normally.

## How often are updates released?

CapivaraOS follows Fedora's cycle. Security and package updates arrive continuously, through the software centre or `dnf`.

## Do I have to reinstall to get the next version?

No, from Series 2 on. The system moves between versions and generations **in place**, without reformatting. On the desktop editions, installed on Btrfs, a snapshot is taken first so you can go back. See [Keeping your system updated](update/index.md) — and, for servers, [Keeping Herd updated](herd/update.md), where the default layout has no automatic snapshot and recovery is from backup.

Series 1 is the exception: it does not have that path and needs a clean install — this is the last time.

## How do I know the ISO I downloaded is really yours?

Every ISO ships with its SHA-256 and with a signature of that checksum, made with our key. The checksum proves the file arrived intact; the signature proves who published it. The steps are in [Verify your download](installation/verify-download.md).

## Does local AI send my data anywhere?

No. The models run on your machine, with no cloud and no account. The only thing that leaves your network is the initial model download. Details in [COS Dev Center](apps/cos-dev.md).

## Where do I report a problem?

On the forum or by opening an issue on GitHub. See the [Contribute](contribute.md) page.

## Does CapivaraOS work in a virtual machine?

Yes, it works in VirtualBox, VMware and QEMU/KVM. For Marsh and Snout, we recommend enabling 3D acceleration.

In a virtual machine, local AI runs on the CPU — it works, but it is slow.

## Can I use CapivaraOS in dual boot?

Yes. The Anaconda installer supports manual partitioning for dual boot with Windows or other Linux distributions.
