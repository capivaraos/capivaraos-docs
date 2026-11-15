# How CapivaraOS is built

This page describes how CapivaraOS is developed: which tools are involved, what
people verify, and what you can check for yourself. It is here for anyone who
needs to assess the system's reliability before putting it to use — it is not
promotional material.

## What you can verify without taking our word for it

| What | Where |
|---|---|
| Desktop edition source | [capivaraos-marsh](https://github.com/capivaraos/capivaraos-marsh), [capivaraos-pup](https://github.com/capivaraos/capivaraos-pup), [capivaraos-snout](https://github.com/capivaraos/capivaraos-snout) |
| Server source | [capivaraos-herd](https://github.com/capivaraos/capivaraos-herd) |
| This documentation | [capivaraos-docs](https://github.com/capivaraos/capivaraos-docs) |
| Change history, with a tracking identifier per item | [Changelog](../changelog.md) |
| Image signatures | `.sha256` and `.sha256.asc` files next to each ISO — see [Verify your download](../installation/verify-download.md) |

The repositories are public and can be cloned anonymously. Where a text of ours
and the code disagree, the code is the source of truth.

## The path a change takes

1. **Branch** off `main`, with a descriptive name.
2. **Pull request**, where the change is read in full before it lands. The
   workflow is written in each repository's `CONTRIBUTING.md` — for example
   [the Herd one](https://github.com/capivaraos/capivaraos-herd/blob/main/CONTRIBUTING.md) —
   and it applies to maintainers as well.
3. **Verification in practice** before publishing: whatever a release starts to
   offer is exercised in a virtual machine or a real install. A feature that has
   not been proven is not announced, and not documented as existing.
4. **A release** is a *tag* plus the published image — never "whatever is on
   `main`".

The private key that signs the packages and the update repository is kept out of
every repository and out of every automation system.

## AI assistants in development

The project uses AI assistants as a working tool. We state it here because you
are entitled to know how the system you install on your machine is made.

!!! info "This is not the product's AI"
    This section is about **how CapivaraOS is developed**. That is a separate
    matter from the **local AI** the system offers its users — models running on
    your own machine, described in [COS Dev Center](../apps/cos-dev.md). Neither
    depends on the other.

**Where they are used:**

- writing and reviewing code and RPM packaging;
- reading security configuration, looking for inconsistencies and mistakes;
- writing and translating this documentation;
- helping prepare and analyse tests.

**What they do not do:**

- they do not approve changes: a person merges every pull request;
- they do not decide what ships to production;
- they do not sign packages and have no access to the private signing key;
- they do not replace the verification on real or virtual hardware described
  above.

Put plainly: AI speeds the work up inside a process that does not accept
unverified output. When a tool and a verification disagree, the verification
wins.

## On Herd (server)

The server runs in other people's production environments, so the bar here is
stricter.

- **Hardening is readable code, not heuristics.** What `herd-harden` applies is
  versioned in the repository — the audit rules, the policies and the script —
  and can be read line by line before you run it. Nothing is decided at runtime
  by a model.
- **Compliance scanning uses third-party content.** The profiles come from the
  SCAP Security Guide, an external and independently audited project; we do not
  write the controls the scan measures.
- **Static analysis in CI.** The Herd repository runs ShellCheck over its shell
  scripts on every push and pull request
  ([workflow](https://github.com/capivaraos/capivaraos-herd/blob/main/.github/workflows/shellcheck.yml)).
- **A published number carries its date, version and method.** Without those, it
  is not published.
- **Precise wording.** We say *compliance-ready* and never *certified*; we say
  *FIPS mode* and never *FIPS validation*. The distinction is set out in
  [Security and compliance](../herd/security.md).

## Found an error?

An error in this documentation or in how the system behaves: open an issue in the
matching repository. A security vulnerability: do **not** open a public issue —
follow that repository's `SECURITY.md`.

---

*Content produced with AI assistance, under continuous review. Found an error? [Open an issue](https://github.com/capivaraos/capivaraos-docs/issues).*
