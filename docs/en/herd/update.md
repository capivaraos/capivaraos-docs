# Keeping Herd updated

An installed Herd receives updates **without reformatting** and without waiting
for a new ISO. There are two kinds of update:

| Kind | What changes | Tool |
|---|---|---|
| **Day-to-day updates** | fixes and new package versions, within the same generation | automatic security updates, plus `dnf upgrade` |
| **Generation upgrade** | jump to the next generation of the system (new Fedora base) | `herd-upgrade` |

!!! note "This page is about Herd"
    The desktop editions update with a different tool. This page covers Herd
    only. For
    desktop, see [Keeping your system updated](../update/index.md).

!!! tip "Coming from Herd 1.0.1?"
    Herd 1.0.1 has no Herd repository and no automatic updates, so it does not
    move to 2.0 on its own.

## Where updates come from

- **Fedora's repositories** (`fedora` and `updates`), for the base system.
- **The Herd repository** (`capivaraos-herd`), for the Herd packages: branding,
  hardening, the update policy and the tools on this page.

dnf checks every package against its signing key before installing it. For the
Herd repository it also checks the signature of the repository index, so a
tampered package list is refused as well. How to check this yourself is in
[Verify the software](security.md#verify-the-software).

## Automatic security updates

Herd installs **security updates on its own**, once a day, and does not reboot
the server. This is done by `dnf5-automatic.timer`, which is on from the first
boot. The policy is in `/etc/dnf/automatic.conf`:

| Setting | Value | Meaning |
|---|---|---|
| `upgrade_type` | `security` | only updates marked as security fixes |
| `apply_updates` | `yes` | download and install them |
| `reboot` | `never` | the server keeps running; you choose when to reboot |

Updates that are not marked as security fixes arrive with `dnf upgrade` (next
section).

Check that it is on and see the last run:

```bash
systemctl is-active dnf5-automatic.timer
journalctl -u dnf5-automatic.service -n 30
```

The result of each run is also shown in the login message.

The file is yours to edit: Herd installs it only when it does not exist yet,
so later Herd updates keep your changes. To turn automatic updates off:

```bash
sudo systemctl disable --now dnf5-automatic.timer
```

!!! note "Reboot after a kernel update"
    A new kernel, or a new version of a library that a running service uses,
    takes effect only after a reboot. Plan reboots in your maintenance window:
    `sudo systemctl reboot`.

!!! warning "The Automatic updates card in Cockpit"
    Cockpit's **Software updates** page shows **Automatic updates: Disabled** on
    Herd, even though `dnf5-automatic.timer` is active. The card does not show
    Herd's policy. Do not change the setting from that card; use the
    commands above.

## Day-to-day updates

To install every available update, not only security fixes:

```bash
sudo dnf upgrade --refresh
```

This covers Fedora's and Herd's repositories. Cockpit's **Software updates**
page does the same from the browser.

## Generation upgrade

`herd-upgrade` checks whether Herd can move to a newer Fedora base. It is built
on dnf's offline upgrade: it downloads with the server running and applies on
the next boot.

```bash
herd-upgrade                  # check only: changes nothing
sudo herd-upgrade download    # download the upgrade; the server keeps running
sudo herd-upgrade reboot      # reboot and apply it
```

By default, `herd-upgrade` refuses to download while the Herd repository has
no packages for the target Fedora release. Herd 2.0 has no newer generation to
upgrade to.

## Snapshots and recovery

This is the part worth planning for.

The usual server layout is **LVM + xfs**, and in that arrangement there is **no
automatic snapshot**: Snapper covers Btrfs, not xfs. Herd also **does not ship
Snapper installed and configured**. Which case applies depends on your install:

- **root on LVM** — no automatic snapshot. A manual snapshot
  (`lvcreate -s …`) depends on **free space in the volume group**. On an
  install where root was grown to fill the disk, there is no space to take the
  snapshot from. `herd-upgrade` prints the command for your system.
- **root on Btrfs with an active Snapper `root` config** — `herd-upgrade` takes
  a snapshot before the upgrade. That requires you to have installed and
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
- [Web console (Cockpit)](cockpit.md)
