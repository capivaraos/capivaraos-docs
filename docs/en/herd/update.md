# Keeping Herd updated

An installed Herd 1.0.1 receives Fedora's updates **without reformatting** and
without waiting for a new ISO. There are two kinds of update:

| Kind | What changes | Tool |
|---|---|---|
| **Day-to-day updates** | fixes and new package versions, within the same generation | `dnf upgrade` |
| **Generation upgrade** | jump to the next generation of the system (new Fedora base) | not included in Herd 1.0.1 |

!!! note "This page is about Herd"
    The desktop editions use `capivaraos-upgrade` and do get an automatic Btrfs
    snapshot. On Herd both of those differ, which is what this page is for. For
    desktop, see [Keeping your system updated](../update/index.md).

## Day-to-day updates

```bash
sudo dnf upgrade --refresh
```

Herd 1.0.1 installs updates only when you run this command; it does not update
on its own. The updates come from Fedora's repositories, and dnf checks every
package from the fedora and updates repositories against Fedora's signing keys
(`gpgcheck`) before installing it. The Herd packages in the 1.0.1 images are not
signed and have no repository, so they are not updated after installation.

## Generation upgrade

Herd 1.0.1 does not include `herd-upgrade`, and there is no Herd repository for
a new generation.

## Snapshots and recovery

This is the part worth planning for.

The usual server layout is **LVM + xfs**, and in that arrangement there is **no
automatic snapshot**: Snapper covers Btrfs, not xfs. Herd also **does not ship
Snapper installed and configured**. Which case applies depends on your install:

- **root on LVM** — no automatic snapshot. A manual snapshot
  (`lvcreate -s …`) depends on **free space in the volume group**. On an
  install where root was grown to fill the disk, there is no space to take the
  snapshot from.
- **root on Btrfs with an active Snapper `root` config** — automatic snapshot,
  as on the desktop editions. That requires you to have installed and
  configured Snapper yourself.
- **neither** — nothing is snapshotted.

On a server, then, **the recovery path is your backup**. Take a verified backup
before a generation upgrade and treat restoring it as the way back.

!!! warning "Do not count on snapshot rollback on Herd"
    Snapshot rollback — and `lvconvert --merge` with it — only works if the
    snapshot exists. On Herd's default layout it does not. Plan with backups.

### A kernel that won't boot is a separate case

A new kernel that fails to boot does not require restoring anything. The system
keeps the **last three** installed kernel versions (dnf's default) and the GRUB
menu lets you boot the previous one. That covers a kernel change, not a
generation upgrade.

## Links

- [Security and compliance](security.md)
- [Administration](administration.md)
- [Download verification](download.md)
