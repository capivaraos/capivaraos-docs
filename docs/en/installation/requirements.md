# System requirements

Figures for **Series 2** (Fedora 45). All three editions are **x86_64**.

## Marsh (KDE Plasma)

| Component | Minimum | Recommended |
|---|---|---|
| CPU | 64-bit dual-core | 64-bit quad-core |
| RAM | 2 GB | 4 GB or more |
| Storage | 20 GB | 40 GB or more |
| Graphics | 3D acceleration support | Dedicated GPU |

## Pup (Xfce)

| Component | Minimum | Recommended |
|---|---|---|
| CPU | 64-bit single-core | 64-bit dual-core |
| RAM | 1 GB | 2 GB or more |
| Storage | 15 GB | 30 GB or more |

## Snout (GNOME)

| Component | Minimum | Recommended |
|---|---|---|
| CPU | 64-bit dual-core | 64-bit quad-core |
| RAM | 2 GB | 4 GB or more |
| Storage | 20 GB | 40 GB or more |
| Graphics | 3D acceleration support | Dedicated GPU |

## Compatibility

CapivaraOS is based on Fedora and inherits its hardware compatibility. Intel and AMD processors are supported, with experimental ARM support.

!!! note "Live mode"
    You can try CapivaraOS without installing by booting from a USB drive. In this case, at least 2 GB of RAM is recommended.

!!! warning "If you plan to use local AI"
    The storage figures above cover the system and ordinary use. The **local AI** in [COS Dev Center](../apps/cos-dev.md) downloads several gigabytes more — add at least **20 GB** to whatever you set aside, and expect more depending on the models you install.

!!! tip "Why Btrfs"
    The installer uses **Btrfs** by default. That is what makes the automatic snapshot before each generation upgrade possible, and it is why the recommended space is more generous than the minimum.
