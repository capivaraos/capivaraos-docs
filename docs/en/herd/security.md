# Security and compliance

HERD is **hardened by default** — a freshly installed server comes with a sensible
security profile, with no configuration needed from you. This page explains
**what's enabled**, how to **audit**, and how to **adjust**.

## What's hardened

| Area | HERD default |
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
    strong by default on Fedora), rather than values hardcoded in HERD.

## Audit: `herd-compliance-scan`

HERD embeds an **OpenSCAP** scanner with Fedora's **SCAP Security Guide** content.
Run:

```bash
sudo herd-compliance-scan
```

This evaluates the system against the **`standard`** profile and generates:

```
/root/herd-compliance-<date>.html   # readable report (open in a browser)
/root/herd-compliance-<date>.xml    # results (machine)
```

To list available profiles or run another one:

```bash
herd-compliance-scan --list
sudo herd-compliance-scan xccdf_org.ssgproject.content_profile_cis
```

!!! tip "Copy the report to your PC"
    ```bash
    scp user@<ip>:/root/herd-compliance-*.html .
    ```
    (or open the file via Cockpit's **Terminal**/**Files**.)

!!! info "Community's baseline profile"
    HERD Community focuses on a solid baseline (SSH, firewall, SELinux, password,
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

## Production best practices

- Expose the minimum: only the necessary ports in `firewalld`.
- Access Cockpit over VPN/tunnel, not open to the internet.
- Keep the system updated (`dnf upgrade`) — Fedora security updates.
- Run `herd-compliance-scan` periodically and address what makes sense for your
  environment.

## Third-party credits and licenses

`herd-compliance-scan` uses the **[SCAP Security Guide](https://github.com/ComplianceAsCode/content)**
(ComplianceAsCode project). HERD redistributes the SSG datastream for Fedora
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
benchmarks; HERD does not reproduce the benchmark text, it only runs the
corresponding profiles shipped by the SSG.

---

*Content produced with AI assistance, under continuous review. Found an error? [Open an issue](https://github.com/capivaraos/capivaraos-herd/issues).*
