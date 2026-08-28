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
| **Packages** | `gpgcheck=1` (signature verification in dnf) |

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

!!! info "Community's baseline profile"
    Herd Community focuses on a solid baseline (SSH, firewall, SELinux, password,
    umask). Heavier controls like **full auditd**, **AIDE** and the complete
    **CIS** profile are the **Enterprise** path; the embedded datastream already
    includes the `cis`/`cis_server_l1` profiles should you want to evaluate them.

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
- Keep the system updated (`dnf upgrade`) — Fedora security updates.
- Run `herd-compliance-scan` periodically and address what makes sense for your
  environment.

## Encryption at rest and FIPS (optional)

Two reinforcements that are **off by default** (so they don't get in the way of
headless servers and cloud), which you enable when your policy calls for them.

### Disk encryption (LUKS)

Protects data **at rest** (a stolen/discarded disk leaks nothing). It's an
**install-time** decision:

- **Recommended:** on the installer's partitioning screen, tick
  **"Encrypt my data"** and set the passphrase.
- **Automated:** the [Herd kickstart](installation-iso.md) ships a commented
  template (`autopart --encrypted`) for an already-encrypted install.

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

!!! note "Fedora 44 no longer ships `fips-mode-setup`"
    Fedora [removed `fips-mode-setup`](https://fedoraproject.org/wiki/Changes/RemoveFipsModeSetup).
    FIPS is now turned on with **`fips=1`** on the kernel command line —
    **preferably at install time** (enabling it afterward "has subtle bugs and is
    no longer recommended", per Fedora). The initramfs already ships the `fips`
    module and the crypto-policy switches to FIPS on its own when the kernel is in
    FIPS mode.

**Recommended — at install:** on the ISO boot menu, edit the entry (`e`/`Tab`)
and append **`fips=1`** to the kernel line. The installed system boots in FIPS.

**Post-install (with the caveat above):**

```bash
sudo grubby --update-kernel=ALL --args="fips=1"
# if /boot is a separate partition, also add: boot=UUID=<uuid-of-/boot>
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
