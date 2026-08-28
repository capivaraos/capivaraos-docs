# One-command hardening

Herd already ships [hardened by default](security.md). When you need to go
**beyond the baseline** — to meet a specific standard like CIS or PCI-DSS —
`herd-harden` applies an entire compliance profile in **one command**.

It's the "apply" counterpart of [`herd-compliance-scan`](security.md#audit-herd-compliance-scan)
(which only *reports*): it uses the same Fedora **SCAP Security Guide** (SSG)
content and applies remediation through **Ansible**.

!!! warning "By default it changes NOTHING"
    Without `--apply`, `herd-harden` runs a **dry-run** (Ansible check mode): it
    shows what *would* change without touching the system. It only applies for
    real with an explicit `--apply`. Always review the dry-run first.

## Available profiles

| Profile | Target | Rules (approx.) | Note |
|---|---|---|---|
| `standard` | Herd baseline (default) | ~76 | Already applied out of the box |
| `ospp` | Protection Profile for general-purpose OS (NIAP/Common Criteria) | ~208 | The "official" (non-draft) profile |
| `cis-l1` | CIS Level 1 — Server | ~324 | SSG draft for Fedora |
| `cis-l2` | CIS Level 2 — Server | ~438 | Draft; more aggressive |
| `pci` | PCI-DSS v3.2.1 | ~121 | Controls for cardholder environments |

You can also pass the **full technical profile id**
(`xccdf_org.ssgproject.content_profile_*`). List them all with `herd-harden --list`.

!!! info "What to expect from each standard — no false promises"
    - **STIG:** there is no DISA STIG profile for Fedora (the SSG only ships it
      for RHEL). If your requirement is DoD-adjacent, **`ospp`** is the closest
      baseline.
    - **CIS:** the CIS profiles for Fedora are community-maintained **drafts** in
      the SSG (CIS does not publish an official Fedora benchmark). Useful and
      solid, but treat them as what they are.
    - **PCI-DSS:** the embedded profile is **v3.2.1**.
    - Herd delivers the **controls and the evidence** — it makes the system
      *compliance-ready*. Compliance itself is process, legal and organization;
      no OS declares itself "certified" on your behalf.

!!! warning "`ospp` and `cis` change the crypto — watch out for ed25519 keys"
    The `ospp` and `cis` profiles enable the **FIPS** *crypto-policy*, which
    **rejects ed25519 SSH keys** (and ciphers outside the FIPS set). On a server
    reached over SSH, make sure you have an **RSA** or **ECDSA** key before
    applying and **test a fresh login** right after `--apply`, without closing
    your current session — `herd-harden` itself warns about this at the end.
    Details on the [Security and compliance](security.md#encryption-at-rest-and-fips-optional)
    page.

## Usage

### 1. See what would change (dry-run)

```bash
sudo herd-harden cis-l1
```

This generates the profile's remediation playbook and runs Ansible in **check
mode** (`--check --diff`): you see every proposed change, nothing is applied.

### 2. Apply

```bash
sudo herd-harden cis-l1 --apply
```

Changes are applied to this server. Output also goes to a log:

```
/var/log/herd-harden-<date>.log
```

### 3. Verify the result

Measure before/after with the scanner, on the **same profile**:

```bash
sudo herd-compliance-scan cis-l1
```

### Other commands

```bash
herd-harden --list      # list the datastream profiles
herd-harden --help      # short help
```

!!! tip "Start with the lightest"
    A good progression is `standard` → `ospp` → `cis-l1` → `pci` (or `cis-l2`,
    the most restrictive). Apply one at a time, always reviewing the dry-run and
    testing your services afterward — higher profiles harden the surface quite a
    lot.

## Cautions

- **Run the dry-run first.** High profiles may restrict something your
  application depends on. Review the `--diff` before `--apply`.
- **Test after applying.** Confirm SSH, your services, and Cockpit.
- **`herd-harden` requires `ansible-core`** (already on Herd). The playbook is
  generated from the vendored datastream — it works offline.
- Specific settings you don't want a profile to overwrite should go in your own
  drop-ins (see [Adjust](security.md#adjust)).

## Third-party credits and licenses

Like `herd-compliance-scan`, `herd-harden` uses Fedora's
**[SCAP Security Guide](https://github.com/ComplianceAsCode/content)**
(`ssg-fedora-ds.xml`, BSD-3-Clause, © 2012-2017 Red Hat, Inc.). License details
on the [Security and compliance](security.md#third-party-credits-and-licenses)
page. Profile names come from the SSG itself; "CIS" refers to the
[Center for Internet Security](https://www.cisecurity.org/) benchmarks — Herd
only runs the corresponding profiles, without reproducing the benchmark text.

---

*Content produced with AI assistance, under continuous review. Found an error? [Open an issue](https://github.com/capivaraos/capivaraos-herd/issues).*
