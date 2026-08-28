# Herd by CapivaraOS

**Herd** is the **server** line of CapivaraOS: a _headless_ Linux
system (no graphical desktop), based on **Fedora 44**, meant to run on physical
servers, virtual machines and the cloud — with a Brazilian identity, sane
security by default, and **Cockpit** as the browser-based administration console.

!!! info "Community Edition 1.0.1"
    This documentation covers **Herd Community**, the **free and open-source**
    edition. Architecture **x86_64** (**aarch64** support arrives in 1.1).

## The idea: herd = fleet

In CapivaraOS, capybaras live in a **herd** — and that is how we think about
servers: a **fleet** that works together. Herd delivers a predictable, hardened
and easy-to-administer server base, whether it is a single node or many.

## What's in Herd Community

| Area | What you get |
|---|---|
| **Base** | Fedora 44 (same generation as the Marsh/Pup/Snout 1.x desktops) |
| **Profile** | _Headless_ server — no desktop, minimal surface |
| **Web console** | **Cockpit** at `https://<ip>:9090` (services, network, storage, accounts, logs, terminal) |
| **Security** | SELinux _enforcing_, restrictive `firewalld`, hardened SSH, password policy, `umask 027` |
| **Compliance** | `herd-compliance-scan` — built-in OpenSCAP report (SSG _standard_ profile) |
| **Cloud** | `cloud-init` on the qcow2 image (key/network/user injection) |
| **Languages** | pt_BR and en_US; ABNT2 keyboard; America/Sao_Paulo timezone (adjustable) |

## Two ways to install

<div class="grid cards" markdown>

-   :material-disc: **Installer ISO (bare-metal)**

    To install on a physical server or VM from scratch, with the Anaconda text
    installer.

    [:octicons-arrow-right-24: Install via ISO](installation-iso.md)

-   :material-cloud: **qcow2 image (cloud/VM)**

    To spin up quickly on cloud or virtualization, with `cloud-init` handling
    the user, SSH key and network.

    [:octicons-arrow-right-24: Cloud/VM deployment](installation-cloud.md)

</div>

## Where to start

1. [Requirements](requirements.md) — what the server needs.
2. [Download and verify](download.md) — get the image and check its integrity.
3. [Install via ISO](installation-iso.md) **or** [cloud/VM](installation-cloud.md).
4. [Getting started](getting-started.md) — access, network, updates.
5. [Web console (Cockpit)](cockpit.md) — administer from the browser.
6. [Security and compliance](security.md) — what's hardened and how to audit.

!!! tip "Already installed?"
    Jump to [Getting started](getting-started.md) or the
    [Web console (Cockpit)](cockpit.md).

## Support and community

Questions, bugs and suggestions: see the [Support](support.md) page. Herd
Community is community-maintained — your participation is welcome.

---

*Content produced with AI assistance, under continuous review. Found an error? [Open an issue](https://github.com/capivaraos/capivaraos-herd/issues).*
