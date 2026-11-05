# Pup

**Desktop:** Xfce 4.20 · **Base:** Fedora 45 · **Architecture:** x86_64

Pup is the light edition. Xfce uses little memory and runs well on older machines, but here it is not a cut-down version: Pup ships the **same development toolkit** as the other two editions.

It is the choice for modest hardware, and also for anyone who simply prefers a more direct desktop.

## What ships with it

- **Xfce 4.20**
- **Firefox** as the default browser
- **LibreOffice**, with Portuguese translation
- **VLC** for video and audio
- **Snapper**, for Btrfs snapshots
- **COS Dev Center** and **COS Config Center**, exclusive to CapivaraOS

Pup is deliberately lean: heavy applications are left out by default and you install what you want.

## For developers

[COS Dev Center](../apps/cos-dev.md) opens at first boot with the same offering as the other editions — local AI, language tracks, dev tools and GitHub.

!!! note "Local AI on a modest machine"
    Local AI downloads several gigabytes and, without a dedicated GPU, runs on the CPU. On an older computer that works, but it is slow: prefer smaller models, or skip this item.

## Visual identity

The same identity as the other editions — Plymouth, GRUB, icons and theme — with a wallpaper collection of our own, including one exclusive to Pup.

## Upgrading

From Series 2 on, Pup upgrades **without reinstalling**. See [Updating the system](../update/index.md).

!!! warning "On Pup, upgrade from the terminal"
    `dnfdragora`, Xfce's default updater, **does not list CapivaraOS updates** on Fedora 45. This is not a problem with our repository — it is `dnfdaemon` failing the signature check. Until that is resolved, use `sudo dnf upgrade` in a terminal. Details in [Updating the system](../update/index.md).

## Requirements

See [Hardware requirements](../installation/requirements.md). Pup asks the least of the three editions.

## Download

[Download Pup](https://capivaraos.org/en/download.html){ .md-button .md-button--primary }

Check the ISO before writing it: [Verify your download](../installation/verify-download.md).
