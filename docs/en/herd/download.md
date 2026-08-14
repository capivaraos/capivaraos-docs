# Download and verify

## Choose the image

HERD Community 1.0.0 is distributed as two **x86_64** images:

| Image | File | When to use |
|---|---|---|
| **Installer ISO** | `CapivaraOS-HERD-1.0.0-x86_64.installer.iso` | Install on a physical server or VM from scratch |
| **qcow2 disk** | `CapivaraOS-HERD-1.0.0-x86_64.qcow2` | Spin up quickly on cloud/virtualization with `cloud-init` |

Download from:

- **Website:** [capivaraos.org — Download](https://capivaraos.org/en/download.html)
- **SourceForge:** [CapivaraOS-HERD/1.0.0 folder](https://sourceforge.net/projects/capivaraos/files/CapivaraOS-HERD/1.0.0/)

## Verify integrity

Always check the **SHA-256** of the downloaded file against the published value.
Both image sums are in the `SHA256SUMS` file, in the same download folder:

```
b97a272ee6f0e91fa609e49d1184e4a4149f61667588a4c13f4ce87c03339123  CapivaraOS-HERD-1.0.0-x86_64.qcow2
9a4c1b9957678e8aa25c1187eb962bcb8460c9415f8b0007de396536deeaa2c3  CapivaraOS-HERD-1.0.0-x86_64.installer.iso
```

=== "Linux / macOS"

    ```bash
    # download SHA256SUMS into the same folder, then run:
    sha256sum -c SHA256SUMS
    # it should answer "OK" for the file(s) you downloaded
    ```

=== "Windows (PowerShell)"

    ```powershell
    Get-FileHash .\CapivaraOS-HERD-1.0.0-x86_64.installer.iso -Algorithm SHA256
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
