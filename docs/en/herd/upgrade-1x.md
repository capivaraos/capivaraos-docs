# Upgrade from Herd 1.0.1

Herd 1.0.1 is based on Fedora 44 and has no Herd repository, so it does not
receive Herd updates. This guide moves an installed Herd 1.0.1 to Herd 2.0,
on Fedora 45, **without reinstalling**.

It takes two stages:

1. move the base from Fedora 44 to Fedora 45;
2. add the signed Herd repository and install the Herd 2.0 packages.

After that, the server has the Herd 2.0 packages, automatic updates are on,
and Herd updates come from the signed repository. See
[Keeping Herd updated](update.md).

!!! warning "Take a backup first"
    A generation upgrade moves the whole base system from Fedora 44 to
    Fedora 45. Take a backup before you start and plan to restore it if the
    upgrade fails. See
    [Snapshots and recovery](update.md#snapshots-and-recovery).

Run every command below on the server, as the administrator user. It needs
internet access.

## Stage 1: Fedora 44 to Fedora 45

Bring Fedora 44 up to date and reboot:

```bash
sudo dnf upgrade --refresh
sudo systemctl reboot
```

While it updates, dnf may ask to import the Fedora 44 signing key, which it
reads from `/etc/pki/rpm-gpg/RPM-GPG-KEY-fedora-44-x86_64`. Answer yes.

Download the Fedora 45 upgrade. The server keeps running while it downloads:

```bash
sudo dnf system-upgrade download --releasever=45
```

dnf asks to confirm the download, then to import the Fedora 45 signing key,
which it reads from a file already on the server
(`/etc/pki/rpm-gpg/RPM-GPG-KEY-fedora-45-x86_64`). Answer yes to both.

Apply it. The server reboots to install the upgrade, which takes a while, and
comes back on Fedora 45:

```bash
sudo dnf offline reboot
```

dnf asks to confirm the reboot.

When it is back, check the base:

```bash
rpm -E %fedora
```

It must print `45`. At this point `/etc/os-release` still says
`VERSION="1.0.1 (Community)"`; stage 2 changes that.

## Stage 2: the Herd repository and the Herd 2.0 packages

### 1. Download the Herd key and check its fingerprint

```bash
curl -fsSLO https://repo-herd.capivaraos.org/RPM-GPG-KEY-capivaraos-herd
gpg --show-keys --with-colons RPM-GPG-KEY-capivaraos-herd | grep -c '^pub'
gpg --show-keys --with-fingerprint RPM-GPG-KEY-capivaraos-herd
```

The second command (`grep -c`) **must print `1`**: the file holds exactly one
key. The third shows that key, and its fingerprint **must be exactly**:

```
C852 6722 0A81 18C9 864F  0648 BE5B 82AD C8A2 85C0
```

!!! danger "If the count is not 1 or the fingerprint does not match"
    Stop here and do not import the file. It is not the Herd key. Report it
    through the
    [private security channel](https://www.capivaraos.org/en/trust-center.html).

When it matches, import it:

```bash
sudo rpmkeys --import RPM-GPG-KEY-capivaraos-herd
```

### 2. Download the repository package and check its signature

```bash
curl -fsSLO https://github.com/capivaraos/repo-herd/releases/download/herd-f45-x86_64-stable/capivaraos-herd-repos-2.0.0-2.fc45.herd.noarch.rpm
LC_ALL=C rpm -K capivaraos-herd-repos-2.0.0-2.fc45.herd.noarch.rpm
rpm -qp --qf '%{RSAHEADER:pgpsig}\n' capivaraos-herd-repos-2.0.0-2.fc45.herd.noarch.rpm
```

The first check must end in `digests signatures OK`, and the second in
`Key ID be5b82adc8a285c0`, the Herd key. `LC_ALL=C` only makes the output
English, to match this text.

!!! warning "Do not skip this check"
    dnf does not check the signature of a package file you install from disk.
    These two lines are the check. Install the file only when both match;
    otherwise stop and report it.

Install it:

```bash
sudo dnf install ./capivaraos-herd-repos-2.0.0-2.fc45.herd.noarch.rpm
```

This adds the Herd repository and the files of the two Herd public keys, the
main key and the reserve key. The first dnf command in the next step asks, once
for each key, whether to import it. Answer yes only if the fingerprints are
`C852 6722 0A81 18C9 864F  0648 BE5B 82AD C8A2 85C0` and
`F4EF 73FA F615 7390 BB65  38FF A765 804F 4ADA 507E` (dnf prints them without
spaces); otherwise answer no and report it.

### 3. Upgrade the Herd packages

```bash
sudo dnf upgrade --refresh
sudo dnf install capivaraos-herd-autoupdate capivaraos-herd-upgrade
sudo systemctl enable --now dnf5-automatic.timer
```

The first command moves the branding and hardening packages to 2.0. The second
adds the two packages that are new in 2.0: the automatic security update
policy and `herd-upgrade`. The third turns on the automatic updates.

## Check the result

```bash
grep -E '^(VERSION|VERSION_ID)=' /etc/os-release
sudo rpm -qa 'capivaraos-herd*' --qf '%{NAME}-%{VERSION}  %{RSAHEADER:pgpsig}\n'
systemctl is-active dnf5-automatic.timer
```

- `VERSION="2.0.0 (Community)"` and `VERSION_ID=45`;
- five `capivaraos-herd-*` packages, all at 2.0.0, each with `Key ID be5b82adc8a285c0`;
- the timer is `active`.

You can now follow the rest of the 2.0 documentation, starting with
[Keeping Herd updated](update.md).
