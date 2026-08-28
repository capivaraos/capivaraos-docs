# Download and verify

## Choose the image

Herd Community 1.0.1 is distributed as two **x86_64** images:

| Image | File | When to use |
|---|---|---|
| **Installer ISO** | `CapivaraOS-HERD-1.0.1-x86_64.installer.iso` | Install on a physical server or VM from scratch |
| **qcow2 disk** | `CapivaraOS-HERD-1.0.1-x86_64.qcow2` | Spin up quickly on cloud/virtualization with `cloud-init` |

Download from:

- **Website:** [capivaraos.org — Download](https://capivaraos.org/en/download.html)
- **SourceForge:** [CapivaraOS-HERD/1.0.1 folder](https://sourceforge.net/projects/capivaraos/files/CapivaraOS-HERD/1.0.1/)

## Verify integrity

Always check the **SHA-256** of the downloaded file against the published value.
Both image sums are in the `SHA256SUMS` file, in the same download folder:

```
395d06472e47b9b572769fe35f295806da01abb6536a89e5b5b82a0ec757776b  CapivaraOS-HERD-1.0.1-x86_64.qcow2
8b5283c9ff2115d30a7983b9641ac15fbfa66b90c57c35ef02d8b0517615d172  CapivaraOS-HERD-1.0.1-x86_64.installer.iso
```

=== "Linux / macOS"

    ```bash
    # download SHA256SUMS into the same folder, then run:
    sha256sum -c SHA256SUMS
    # it should answer "OK" for the file(s) you downloaded
    ```

=== "Windows (PowerShell)"

    ```powershell
    Get-FileHash .\CapivaraOS-HERD-1.0.1-x86_64.installer.iso -Algorithm SHA256
    # compare the output with the value above
    ```

!!! warning "If the checksum doesn't match"
    Do not use the image. A different hash means a corrupted download or a
    tampered file — download it again, preferably from the official source.

## Next step

- Physical server/VM → [Install via ISO](installation-iso.md)
- Cloud/virtualization → [Cloud/VM deployment](installation-cloud.md)

---

*Content produced with AI assistance, under continuous review. Found an error? [Open an issue](https://github.com/capivaraos/capivaraos-herd/issues).*
