# Download and verify

## Choose the image

Herd Community 2.0.0 is distributed as two **x86_64** images:

| Image | File | When to use |
|---|---|---|
| **Installer ISO** | `CapivaraOS-HERD-2.0.0-x86_64.installer.iso` | Install on a physical server or VM from scratch |
| **qcow2 disk** | `CapivaraOS-HERD-2.0.0-x86_64.qcow2` | Spin up quickly on cloud/virtualization with `cloud-init` |

Download from:

- **Website:** [capivaraos.org — Download](https://capivaraos.org/en/download.html)
- **SourceForge:** [CapivaraOS-HERD/2.0.0 folder](https://sourceforge.net/projects/capivaraos/files/CapivaraOS-HERD/2.0.0/)

The same folder has `SHA256SUMS`, with the checksums of both images, and
`SHA256SUMS.asc`, its signature. Download both with the image.

## 1. Is the file intact? (checksum)

```
0fac4847ccb6950803aa6c18a9d8daaacbea1f2b0ce728ed532945b825bbe375  CapivaraOS-HERD-2.0.0-x86_64.qcow2
c0934b5fe32999d422c4f14002b94f1c08d54456d83c441b9b6ad1597ff97ac9  CapivaraOS-HERD-2.0.0-x86_64.installer.iso
```

=== "Linux / macOS"

    ```bash
    # SHA256SUMS in the same folder as the image, then run:
    sha256sum -c --ignore-missing SHA256SUMS
    # it should answer "OK" for the file(s) you downloaded
    ```

=== "Windows (PowerShell)"

    ```powershell
    Get-FileHash .\CapivaraOS-HERD-2.0.0-x86_64.installer.iso -Algorithm SHA256
    # compare the output with the value above
    ```

    This checks that the file is intact. To also check that it is ours
    (step 2), use GnuPG for Windows (Gpg4win) or a Linux machine.

!!! warning "If the checksum doesn't match"
    Do not use the image. A different hash means a corrupted download or a
    tampered file — download it again, preferably from the official source.

## 2. Is the file ours? (signature)

The checksum proves the file arrived intact, but not **who** published it:
anyone who swapped the image would publish a matching checksum too. The
signature answers that. `SHA256SUMS.asc` is the GPG signature of `SHA256SUMS`,
made with the Herd key, the same key that signs the Herd updates.

**a) Download the Herd public key and check it:**

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

!!! danger "If the count is not 1 or the fingerprint doesn't match"
    Stop. That file is not the Herd key. Don't use the image and report it
    through the
    [security channel](https://www.capivaraos.org/en/trust-center.html).

**b) Verify the signature against that key alone** (all files in the same
folder):

```bash
gpg --dearmor < RPM-GPG-KEY-capivaraos-herd > herd-key.gpg
LC_ALL=C gpgv --keyring ./herd-key.gpg SHA256SUMS.asc SHA256SUMS
# expected: Good signature from "CapivaraOS Herd (repo signing key) ..."
```

`gpgv` checks the signature only against the key you checked in step (a), so a
**Good signature** here means `SHA256SUMS` was signed with the Herd key.
`LC_ALL=C` only makes the output English, to match this text.

Then run the checksum check in step 1 against this `SHA256SUMS`.

!!! note "The Herd key is separate from the desktop key"
    The desktop editions are signed with a different key. Use the Herd key for
    Herd images and updates.

## Next step

- Physical server/VM → [Install via ISO](installation-iso.md)
- Cloud/virtualization → [Cloud/VM deployment](installation-cloud.md)
- Already running Herd 1.0.1 → [Upgrade from Herd 1.0.1](upgrade-1x.md)

---

*Content produced with AI assistance, under continuous review. Found an error? [Open an issue](https://github.com/capivaraos/capivaraos-herd/issues).*
