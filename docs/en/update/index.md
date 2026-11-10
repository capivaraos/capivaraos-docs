# Keeping your system updated

An installed CapivaraOS stays up to date **without reformatting** and without
waiting for a new ISO. There are two kinds of update, with different tools:

| Kind | What changes | Tool |
|---|---|---|
| **Day-to-day updates** | fixes and new program versions, within the same generation | the app store or `dnf upgrade` |
| **Generation upgrade** | jump to the next generation of the system (new Fedora base) | `capivaraos-upgrade` |

!!! note "This page is about the desktop editions"
    Marsh, Pup and Snout use `capivaraos-upgrade` and do get an automatic Btrfs
    snapshot. **Herd** uses `herd-upgrade` and, on the usual server layout
    (LVM + xfs), takes **no** automatic snapshot — see
    [Keeping Herd updated](../herd/update.md).

In both, besides Fedora's packages you receive **our** packages (branding, COS
Dev Center, themes, fixes) from a **signed repository**. Trust doesn't depend on
trusting the server: every package **and** the metadata are verified against our
GPG key (`gpgcheck` + `repo_gpgcheck`). The private half of that key is kept in a
vault and only enters a machine at the moment of signing a release. It's the same
key from [verifying your download](../installation/verify-download.md).

## Day-to-day updates

These arrive on their own through each edition's app store — just apply them when
they show up:

| Edition | Where |
|---|---|
| **Marsh** (KDE) | **Discover** |
| **Snout** (GNOME) | **GNOME Software** |
| **Pup** (Xfce) | from the terminal — see the warning below |

From the terminal, on any edition, this always works:

```bash
sudo dnf upgrade --refresh
```

!!! warning "Pup: update from the terminal"
    On the Fedora 45 base, Xfce's graphical updater may **not list** our updates
    (a limitation of that component, not of our repository). On Pup, prefer
    `sudo dnf upgrade --refresh` — it's the reliable path until the graphical
    updater is fixed upstream.

## Generation upgrade (in-place)

When a **new generation** of CapivaraOS is published, you jump to it without
reinstalling, using `capivaraos-upgrade`. It uses `dnf5`'s **offline** transaction
(`system-upgrade`): it downloads everything while the system is running and
applies it on the next boot, in an isolated environment.

It's built not to surprise you: by default it only **checks**, and downloading
and rebooting are **separate, explicit** steps.

```bash
# 1. Pre-flight: shows what would happen. Downloads and reboots nothing.
capivaraos-upgrade

# 2. Download the transaction (system stays up; no reboot):
sudo capivaraos-upgrade download

# 3. Apply: reboots and performs the upgrade in the offline environment.
sudo capivaraos-upgrade reboot
```

!!! info "Fails closed: it won't leave you half-upgraded"
    The upgrade only proceeds if **our signed repository already exists** for the
    target generation. If it doesn't yet, it **refuses** — otherwise the machine
    would move to the new Fedora base and leave the CapivaraOS packages
    (branding, COS Dev, themes) behind. So running `capivaraos-upgrade` **today**
    will usually answer that no target is published yet: that's the correct
    behavior, not an error.

### Before the upgrade: a snapshot

When the disk supports it, the tool takes a **snapshot first**, so you can roll
back:

- **btrfs + snapper**: **automatic** snapshot. This is the installer's default
  arrangement on all three editions.
- **LVM**: not automatic — the tool prints the manual command
  (`lvcreate -s …`) in the pre-flight.
- **No compatible storage**: nothing is snapshotted — **back up** first.

!!! tip "The pre-flight tells you what will happen on your machine"
    No need to guess which case you are in: run `capivaraos-upgrade` with no
    arguments. It reports whether a snapshot will be taken, and under what
    conditions, before anything is downloaded.

### If you need to go back

The snapshot the tool creates shows up in the list described as
**"pré capivaraos-upgrade"** — that is your return point:

```bash
sudo snapper list
```

To undo the file changes from that point to now:

```bash
sudo snapper undochange <snapshot#>..0
```

The `0` means "the current state". To revert **the whole system** after a
generation upgrade, the path is to **boot the snapshot and promote it** —
`undochange` reverts file changes, it does not swap the system over.

On **LVM**, the rollback is `sudo lvconvert --merge <snapshot>` followed by a
reboot — but only if an LVM snapshot was created beforehand, by hand. The LVM
snapshot is not automatic and needs free space in the volume group.

## Why Series 1 doesn't upgrade to Series 2

Series 1 has reached **end of life** and has neither the signed repository nor
the in-place upgrade engine (both were born in Series 2). There is therefore no
in-place jump from Series 1 to Series 2: if you're on Series 1, do a **clean
install** of Series 2 — see [verifying your download](../installation/verify-download.md)
and the [installation guide](../installation/guide.md). Back up your data first.
The Series 1 documentation is still available in the
[Archive](../arquivo/1.x/index.md).
