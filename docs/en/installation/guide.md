# Installation guide

## 1. Download the ISO

Go to the [download page](https://capivaraos.org/en/download.html) and choose your distro: Marsh, Pup or Snout.

## 2. Flash to a USB drive

You'll need a USB drive with at least 4 GB. We recommend **Fedora Media Writer** or **balenaEtcher** — they write the image straight to the drive and just work:

- **Fedora Media Writer** (Windows, macOS, Linux) — Fedora's official tool, the best choice
- **balenaEtcher** (Windows, macOS, Linux) — simple, three clicks
- **dd** (Linux/macOS, via terminal):

```bash
sudo dd if=capivaraos-marsh-2.0.0.iso of=/dev/sdX bs=4M status=progress
```

!!! warning "Be careful with dd"
    Check the correct device with `lsblk` before running the command. Overwriting the wrong device is irreversible.

!!! note "About Ventoy"
    Ventoy (which keeps several ISOs on one stick) may **fail to boot** Fedora/CapivaraOS live images on some machines: it loads the ISO over *loopback* and the live system can't always find its root. If you do use it, update Ventoy to the latest version, enable **"Ventoy Compatible Mode"**, verify the **SHA-256 of the ISO already copied** onto the stick, and with **Secure Boot** on, enroll Ventoy's key (MOK) when prompted. When in doubt, use Fedora Media Writer.

## 3. Boot from USB

Restart your computer and enter the boot menu (usually F12, F2 or Del during startup). Select the USB drive.

## 4. Try or install

When it boots, you'll see the option to enter live mode (try without installing) or start the installation directly.

## 5. Follow the Anaconda installer

CapivaraOS uses the Anaconda installer, the same one used by Fedora. It will guide you through:

1. Language selection (pre-configured as Brazilian Portuguese)
2. Disk partitioning
3. User account creation
4. Installation

## 6. Done

After installation, remove the USB drive and reboot. The system comes pre-configured in Portuguese, with the desktop environment ready to use.

---

Hit a problem? Post on the [forum](https://capivaraos.org) or open an issue on GitHub.
