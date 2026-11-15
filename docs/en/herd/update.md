# Keeping Herd updated

An installed Herd stays up to date **without reformatting** and without waiting
for a new ISO, from the same signed repository as the other editions. There are
two kinds of update, with different tools:

| Kind | What changes | Tool |
|---|---|---|
| **Day-to-day updates** | fixes and new package versions, within the same generation | `dnf upgrade` |
| **Generation upgrade** | jump to the next generation of the system (new Fedora base) | `herd-upgrade` |

!!! note "This page is about Herd"
    The desktop editions use `capivaraos-upgrade` and do get an automatic Btrfs
    snapshot. On Herd both of those differ, which is what this page is for. For
    desktop, see [Keeping your system updated](../update/index.md).

## Day-to-day updates

```bash
sudo dnf upgrade --refresh
```

Besides Fedora's packages you receive **ours** from a **signed repository**:
every package **and** the metadata are verified against our GPG key
(`gpgcheck` + `repo_gpgcheck`). A signature that does not check out **stops the
transaction** — there is no path that installs one of our packages without
passing verification.

## Generation upgrade

`herd-upgrade` drives `dnf5 system-upgrade` in explicit steps:

```bash
herd-upgrade                   # pre-flight: shows what would happen; changes nothing
sudo herd-upgrade download     # downloads the offline transaction (no reboot)
sudo herd-upgrade reboot       # applies now, rebooting into offline mode
```

With no argument it runs the pre-flight and **nothing is changed**. The default
target is the current generation + 1; for another, use `--to N`.

!!! tip "Run the pre-flight first"
    The pre-flight answers, for **your** machine, whether a target generation
    has been published, what filesystem the root is on, and whether a snapshot
    will be taken. You don't have to guess which case you are in.

If no target generation has been published yet, the pre-flight **refuses** —
otherwise the machine would move to a new Fedora base and leave the CapivaraOS
packages behind. That is the correct behaviour, not a failure.

## Snapshots and recovery

This is the part worth planning for.

The usual server layout is **LVM + xfs**, and in that arrangement there is **no
automatic snapshot**: Snapper covers Btrfs, not xfs. Herd also **does not ship
Snapper installed and configured**. `herd-upgrade` detects the case and tells
you in the pre-flight which one you are in:

- **root on LVM** — no automatic snapshot. The tool prints the manual command
  (`lvcreate -s …`), which depends on **free space in the volume group**. On an
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

---

*Content produced with AI assistance, under continuous review. Found an error? [Open an issue](https://github.com/capivaraos/capivaraos-herd/issues).*
