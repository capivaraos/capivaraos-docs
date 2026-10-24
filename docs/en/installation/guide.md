# Installation guide

## 1. Download the ISO

Go to the [download page](https://capivaraos.org/en/download.html) and choose your distro: Marsh, Pup or Snout.

## 2. Flash to a USB drive

You'll need a USB drive with at least 4 GB. Use one of these tools:

- **Balena Etcher** (Windows, macOS, Linux) — easiest option
- **Ventoy** (Windows, Linux) — lets you keep multiple ISOs on the same drive
- **dd** (Linux/macOS, via terminal):

```bash
sudo dd if=capivaraos-marsh-1.x.x.iso of=/dev/sdX bs=4M status=progress
```

!!! warning "Be careful with dd"
    Check the correct device with `lsblk` before running the command. Overwriting the wrong device is irreversible.

!!! note "If Ventoy hangs at startup"
    Ventoy works in most cases, but on some machines the live session won't come up — the boot gets stuck at *"Starting CapivaraOS…"*, sometimes with a message like `Can't create /lib/dracut/hooks/.../99-ventoy-…sh`. That error comes from Ventoy itself (it injects that script at boot), not from the ISO. If this happens, **write the ISO directly to the USB drive** with Balena Etcher, Fedora Media Writer or `dd` (without Ventoy) and boot again.

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
