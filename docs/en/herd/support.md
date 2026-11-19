# Support and lifecycle

## Edition

**Herd Community** is **free and open-source** (the code under GPLv3; the name and
logo are the project's trademark). It is community-maintained, on a **best-effort**
basis — with no contractual SLA.

## Lifecycle

- Herd **2.x** is based on **Fedora 45** and follows that base's updates.
  Security updates are installed automatically every day; see
  [Keeping Herd updated](update.md).
- Herd **1.0.1** is based on Fedora 44 and receives no Herd updates. Move it to
  2.0 with [Upgrade from Herd 1.0.1](upgrade-1x.md).

!!! tip "Keep it updated"
    The best security posture is an up-to-date system. Leave the automatic
    security updates on, run `dnf upgrade` regularly (or use the
    **Software updates** page in [Cockpit](cockpit.md)), and plan reboots.

## Report bugs

Found a problem? Open an issue on GitHub:

- **[github.com/capivaraos/capivaraos-herd/issues](https://github.com/capivaraos/capivaraos-herd/issues)**

It helps a lot to include:

- version (`cat /etc/os-release`);
- how to reproduce;
- relevant logs (`journalctl -b -p err`, or from the affected service);
- if applicable, the output of `herd-compliance-scan`.

## Community

- **Website:** [capivaraos.org](https://capivaraos.org)
- **News:** [capivaraos.org/en/blog.html](https://capivaraos.org/en/blog.html)
- **Social:** Instagram, X and YouTube (`@CapivaraOS` / `@Capivara-OS`)

## Contribute

CapivaraOS is an open Brazilian project. Bug reports, documentation improvements,
testing and spreading the word are all welcome — see the
[Contribute](../contribute.md) page.

## FAQ

??? question "Can I log in over SSH with a password right after installing?"
    No — SSH is **key-only** (`PasswordAuthentication no`). Add your key via the
    console/Cockpit, or (with caveats) enable password auth per
    [Security › SSH](security.md#ssh).

??? question "Does Cockpit accept root login?"
    No. Use the administrator user (`wheel` group), then click
    **Limited access** to switch to administrative access.

??? question "Is there an ARM (aarch64) version?"
    No. Herd 2.0 ships for x86_64 only.

??? question "Can I use it in commercial production?"
    Yes. The software is free (GPLv3) and can be used for any purpose, including
    commercial. Only the **trademark** (name/logo) is protected — see the
    repository's `TRADEMARK.md`.

??? question "How do I set up a web server/database/container?"
    Herd is a lean server base. Install what you need with `dnf` (`nginx`,
    `podman`, `postgresql-server`, etc.) and open the ports in the
    [firewall](administration.md#firewall).

---

*Content produced with AI assistance, under continuous review. Found an error? [Open an issue](https://github.com/capivaraos/capivaraos-herd/issues).*
