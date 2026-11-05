# Installation guide

## 1. Download the ISO

Go to the [download page](https://capivaraos.org/en/download.html) and choose your distro: [Marsh](../distros/marsh.md), [Pup](../distros/pup.md) or [Snout](../distros/snout.md).

## 2. Check what you downloaded

Before writing it, check the ISO's SHA-256. It is quick, and it rules out the most common reason for a USB stick that will not boot: a corrupted download.

If you want to be sure not only that the file arrived intact but that it is **ours**, check the signature too. The steps are in [Verify your download](verify-download.md).

## 3. Flash to a USB drive

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

## 4. Boot from USB

Restart your computer and enter the boot menu (usually F12, F2 or Del during startup). Select the USB drive.

In the menu you will see **CapivaraOS \<edition\> 2 45** — the 2 is the CapivaraOS version and the 45 is the Fedora release it is built on.

## 5. Try or install

When it boots, you'll see the option to enter live mode (try without installing) or start the installation directly.

!!! tip "Try it first, if you can"
    Live mode runs from memory and does not touch your disk. It is how you confirm that wireless, sound and video work on your machine before installing anything.

## 6. Follow the installer

CapivaraOS uses the Anaconda installer, the same one used by Fedora. It will guide you through:

1. **Language** — the system ships ready in Brazilian Portuguese and English; you pick during installation and can change it later in [COS Config Center](../apps/cos-config.md)
2. **Disk partitioning** — the default is Btrfs, which is what makes the snapshots before each upgrade possible
3. **User account creation**
4. **Installation**

!!! note "Encrypting the disk"
    If you choose encryption (LUKS), the computer will ask for the disk password at every boot, on a screen with the capybara. Keep that password: without it there is no way to open the disk.

## 7. First boot

Remove the USB drive and reboot. At first login **[COS Dev Center](../apps/cos-dev.md)** opens by itself and asks what you want to install — local AI, languages, dev tools. None of it is mandatory: you can close it and open it later from the menu.

From then on the system stays up to date without reinstalling. See [Keeping your system updated](../update/index.md).

---

Hit a problem? Post on the [forum](https://capivaraos.org) or open an issue on GitHub.
