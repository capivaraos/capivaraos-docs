# Security and compliance

Herd is **hardened by default** — a freshly installed server comes with a sensible
security profile, with no configuration needed from you. This page explains
**what's enabled**, how to **audit**, and how to **adjust**.

## What's hardened

| Area | Herd default |
|---|---|
| **SELinux** | `enforcing` |
| **Firewall** | `firewalld` allowing only **SSH** and **Cockpit** (plus `dhcpv6-client`); `mdns` off |
| **SSH** | key-only; no root; login attempt limits (see below) |
| **Passwords** | quality minimum (`pwquality`) and password aging |
| **umask** | `027` on login sessions (new files aren't world-readable) |
| **Packages** | signed Herd repository; dnf checks packages and the repository index (`gpgcheck`, `repo_gpgcheck`) |
| **Updates** | security updates installed automatically every day, no automatic reboot (see [Keeping Herd updated](update.md)) |

### SSH — default policy

File: `/etc/ssh/sshd_config.d/50-capivaraos-herd.conf`

```text
PermitRootLogin no
PasswordAuthentication no
PermitEmptyPasswords no
KbdInteractiveAuthentication no
X11Forwarding no
MaxAuthTries 3
LoginGraceTime 30
AllowAgentForwarding no
```

### Password policy

- Quality — `/etc/security/pwquality.conf.d/50-capivaraos-herd.conf`: minimum
  length **12**, at least **3 character classes**, no long repeats/sequences.
- Aging — `/etc/login.defs`: maximum **90** days, minimum **7** days.

!!! note "Ciphers (crypto policy)"
    SSH and TLS ciphers/MACs/KEX follow the **system crypto-policy** (already
    strong by default on Fedora), rather than values hardcoded in Herd.

## Audit: `herd-compliance-scan`

Herd embeds an **OpenSCAP** scanner with Fedora's **SCAP Security Guide** content.
Run:

```bash
sudo herd-compliance-scan
```

This evaluates the system against the **`standard`** profile and generates:

```
/root/herd-compliance-<date>.html      # readable report (open in a browser)
/root/herd-compliance-<date>.xml       # XCCDF results (machine)
/root/herd-compliance-<date>.arf.xml   # ARF: re-renderable evidence / auditing
```

To list available profiles or run another one (it accepts the **same aliases**
as [`herd-harden`](hardening.md) — `standard`, `ospp`, `cis-l1`, `cis-l2`, `pci`
— or the full id):

```bash
herd-compliance-scan --list
sudo herd-compliance-scan ospp
```

!!! tip "ARF is the evidence"
    The `.arf.xml` is the *machine-readable* format that bundles datastream +
    results together: you can **regenerate the report later** without re-scanning,
    and it's what auditing/aggregation tools consume.

!!! tip "Copy the report to your PC"
    ```bash
    scp user@<ip>:/root/herd-compliance-*.html .
    ```
    (or open the file via Cockpit's **Terminal**/**Files**.)

!!! info "Why `standard` is the default"
    The `standard` profile is a balanced baseline: it closes what matters (SSH,
    firewall, SELinux, password, umask) without breaking ordinary services. That
    is why it runs by default.

    The stricter profiles — `cis-l1`, `cis-l2`, `ospp` and `pci` — **ship with
    the system and run with one command**, no licence and nothing to install:

    ```bash
    sudo herd-harden cis-l2        # dry-run by default
    sudo herd-harden cis-l2 --apply
    ```

    They change a great deal more (CIS Level 2 evaluates 438 rules against 75 in
    `standard`) and can break services you rely on. Run the dry-run first, read
    what would change, and decide — it is your call, not an edition limit.

## Verify the software

The image checksum and its signature prove the image is intact and ours — see
[Download and verify](download.md). After installing, these four checks show
that the system trusts the right keys and requires their signatures.
You run all of them yourself.

### 1. Are the keys installed?

The Herd public keys ship inside the system, in the `capivaraos-herd-repos`
package:

```bash
sudo rpm -q gpg-pubkey --qf '%{VERSION}  %{SUMMARY}\n' | grep -i capivaraos
```

It must list exactly two keys, the main key and the reserve key. Each line
starts with the key's fingerprint from step 2, in lowercase and without spaces.
No other key may have a CapivaraOS name. If the list is empty on a new server,
do check 2 first, then run `sudo systemctl start dnf5-automatic.service` once
(this also installs any pending security updates) and check again.

### 2. Do the fingerprints match?

This is the step that matters. Compare what is on your machine against the
values published here:

```bash
gpg --show-keys --with-fingerprint /etc/pki/rpm-gpg/RPM-GPG-KEY-capivaraos-herd
```

The file must hold one key, and its fingerprint **must be exactly**:

```
C852 6722 0A81 18C9 864F  0648 BE5B 82AD C8A2 85C0
```

The system also ships a reserve key, used to sign only when the main key is
replaced, in a planned rotation or after a compromise. Check it the same way:

```bash
gpg --show-keys --with-fingerprint /etc/pki/rpm-gpg/RPM-GPG-KEY-capivaraos-herd-reserve
```

It must also hold one key, and its fingerprint **must be exactly**:

```
F4EF 73FA F615 7390 BB65  38FF A765 804F 4ADA 507E
```

!!! danger "If either does not match"
    Stop. A different key means the system trusts someone who is not us. Do not
    apply updates, and report it through the
    [private security channel](https://www.capivaraos.org/en/trust-center.html).

The Herd key is **separate** from the one used by the desktop editions
(`75FA 3340 40FD 0150 4AAD  9337 E07F 78EA 4070 11BD`). Compromising one does
not compromise the other.

### 3. Are the installed packages signed?

```bash
rpm -q --qf '%{NAME}: %{RSAHEADER:pgpsig}\n' capivaraos-herd-branding capivaraos-herd-hardening
```

Each line must end in `Key ID be5b82adc8a285c0`. To check a package file
before installing it:

```bash
LC_ALL=C rpm -K ./some-package.rpm
# expected: ...: digests signatures OK
```

For a package that was altered, or signed by a key your system does not trust,
`rpm -K` shows `SIGNATURES NOT OK`.

### 4. Does the repository require signatures?

```bash
grep -E 'gpgcheck|repo_gpgcheck' /etc/yum.repos.d/capivaraos-herd.repo
```

Every line must end in `1`. `gpgcheck` checks each package; `repo_gpgcheck`
also checks the repository **index** — without it, someone controlling the
server could advertise a different package list, even without being able to
sign the packages themselves. Fedora's repositories use `gpgcheck=1` as well.

!!! warning "`makecache` is not the test"
    On dnf5, `dnf makecache` only **warns** when metadata is badly signed, and
    still exits 0. The **transaction** is what refuses it. To test for real,
    try an actual operation (`dnf check-update` or `dnf upgrade`) — that is
    where a tampered repository is rejected.

### Why this holds even if the server falls

Every package and the repository index are checked against the Herd public
keys installed on your system. A file altered on the repository server, or on
a CDN in front of it, no longer matches its signature, so your system refuses
it. That is why the trust sits in the key, not in whoever hosts it.

## Adjust

All configuration comes from the `capivaraos-herd-hardening` package. You can
override it to match your policy.

### <a id="ssh"></a>SSH — enable password (with a caveat)

!!! warning "Think twice"
    SSH password authentication increases the attack surface (brute force).
    Prefer **keys**. Only enable password auth if you really need it, and combine
    it with `fail2ban`/firewall source limits.

Create a _drop-in_ that comes **after** ours (alphabetical order):

```bash
sudo tee /etc/ssh/sshd_config.d/60-local.conf >/dev/null <<'EOF'
PasswordAuthentication yes
EOF
sudo systemctl restart sshd
```

### Adjust password rules

```bash
sudo tee /etc/security/pwquality.conf.d/60-local.conf >/dev/null <<'EOF'
minlen = 14
EOF
# aging: edit /etc/login.defs (PASS_MAX_DAYS / PASS_MIN_DAYS)
```

### Add your SSH key

```bash
install -d -m 700 ~/.ssh
echo "ssh-ed25519 AAAA... you@machine" >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```

!!! warning "Planning to apply `ospp`/`cis` or FIPS mode? Don't rely on ed25519"
    These profiles switch the system *crypto-policy* to **FIPS**, which **does not
    accept ed25519 keys** (nor ciphers outside the FIPS set). Generate an **RSA**
    (`ssh-keygen -t rsa -b 3072`) or **ECDSA** (`ssh-keygen -t ecdsa -b 384`) key
    **before** hardening and confirm a fresh login. For the default baseline,
    ed25519 is still great. See [One-command hardening](hardening.md).

## Production best practices

- Expose the minimum: only the necessary ports in `firewalld`.
- Access Cockpit over VPN/tunnel, not open to the internet.
- Keep automatic security updates on, run `dnf upgrade` regularly and plan
  reboots — see [Keeping Herd updated](update.md).
- Run `herd-compliance-scan` periodically and address what makes sense for your
  environment.

## Encryption at rest and FIPS (optional)

Two reinforcements that are **off by default** (so they don't get in the way of
headless servers and cloud), which you enable when your policy calls for them.

### Disk encryption (LUKS)

Protects data **at rest** (a stolen/discarded disk leaks nothing). It's an
**install-time** decision. Since the Herd installer is **text-mode** (no "Encrypt
my data" checkbox from the graphical version), encryption is set via the
**kickstart** — the `cryptsetup` package ships in the ISO payload,
so an encrypted install works offline. Use the [Herd kickstart](installation-iso.md)
template:

```text
clearpart --all --initlabel
autopart --type=lvm --encrypted --passphrase=YOUR-PASSPHRASE
```

Point the installer at that kickstart either by building your own ISO
(`KS=my.ks ./build-iso.sh`, see the [repository](https://github.com/capivaraos/capivaraos-herd))
or by passing `inst.ks=<url>` at boot.

Verify afterward:

```bash
lsblk                       # the device should show up as "crypt"
sudo cryptsetup status luks-<uuid>
```

!!! warning "Headless server: the passphrase is asked at boot"
    A LUKS volume prompts for the **passphrase on every boot**. Without a console
    (physical/IPMI/serial), the machine **won't come up on its own** after a
    reboot. For server auto-unlock, use **NBDE** — `clevis` with **TPM2** (local
    seal) or **Tang** (key over the network). **Do not** use LUKS on the **cloud**
    image (qcow2).

### FIPS mode

FIPS 140 forces the use of validated cryptographic modules only and restricts the
allowed algorithms. Useful for government/sector requirements.

!!! warning "FIPS mode is not FIPS validation"
    Turning on `fips=1` puts the system in **FIPS mode**: it uses only the
    cryptographic modules the base ships for that mode and rejects algorithms
    outside the allowed set. That is a **configuration**, not a validation:
    neither Herd nor the base becomes "FIPS certified" by enabling the mode.
    CMVP validation is a formal process, carried out by an accredited laboratory
    against a specific version of a module. If your requirement calls for a
    **CMVP-validated** module, check the exact version you run against
    [NIST's CMVP](https://csrc.nist.gov/projects/cryptographic-module-validation-program/validated-modules)
    — the mode being on does not prove validation. See also the compliance
    caveat in [One-command hardening](hardening.md).

!!! note "Fedora no longer ships `fips-mode-setup`"
    Fedora [removed `fips-mode-setup`](https://fedoraproject.org/wiki/Changes/RemoveFipsModeSetup).
    FIPS is now turned on with **`fips=1`** on the kernel command line —
    **preferably at install time** (enabling it afterward "has subtle bugs and is
    no longer recommended", per Fedora). The initramfs already ships the `fips`
    module and the crypto-policy switches to FIPS on its own when the kernel is in
    FIPS mode.

**Recommended — at install:** on the ISO boot menu, edit the entry (`e`/`Tab`)
and append **`fips=1`** to the kernel line. The installed system boots in FIPS.

**Post-install (with the caveat above):** on Herd `/boot` is a separate
partition, so `fips=1` must be paired with `boot=UUID=<uuid of /boot>`. This
block detects the UUID for you:

```bash
if findmnt /boot >/dev/null 2>&1; then
  UUID=$(sudo blkid -s UUID -o value "$(findmnt -no SOURCE /boot)")
  ARGS="fips=1 boot=UUID=$UUID"
else
  ARGS="fips=1"
fi
sudo grubby --update-kernel=ALL --args="$ARGS"
sudo reboot
```

Verify:

```bash
cat /proc/sys/crypto/fips_enabled     # 1 = active
update-crypto-policies --show          # FIPS
```

To disable: remove `fips=1`
(`sudo grubby --update-kernel=ALL --remove-args="fips=1"`) and reboot.

!!! warning "Test access before relying on it"
    FIPS mode **restricts algorithms**: in particular, **ed25519 SSH keys stop
    being accepted** (use **RSA** or **ECDSA**), along with ciphers and
    certificates outside the approved set. **Confirm SSH login on a second
    session** right after rebooting, before closing the access you have.

## Third-party credits and licenses

`herd-compliance-scan` uses the **[SCAP Security Guide](https://github.com/ComplianceAsCode/content)**
(ComplianceAsCode project). Herd redistributes the SSG datastream for Fedora
(`ssg-fedora-ds.xml`), licensed under **BSD-3-Clause**:

> Copyright (c) 2012-2017, Red Hat, Inc. All rights reserved.
>
> Redistribution and use in source and binary forms, with or without
> modification, are permitted provided the BSD-3-Clause conditions are met
> (retain the copyright notice, conditions and disclaimer; the holders' names
> may not be used for endorsement). The full text ships with the
> `capivaraos-herd-hardening` package under
> `/usr/share/licenses/capivaraos-herd-hardening/` and is available in the
> [SSG repository](https://github.com/ComplianceAsCode/content/blob/master/LICENSE).

Profile names (`standard`, `cis`, `cis_server_l1`, etc.) come from the SSG
itself. "CIS" refers to the [Center for Internet Security](https://www.cisecurity.org/)
benchmarks; Herd does not reproduce the benchmark text, it only runs the
corresponding profiles shipped by the SSG.

---

*Content produced with AI assistance, under continuous review. Found an error? [Open an issue](https://github.com/capivaraos/capivaraos-herd/issues).*
