# Download and verify

## Choose the image

HERD Community 1.0.0 is distributed as two **x86_64** images:

| Image | File | When to use |
|---|---|---|
| **Installer ISO** | `CapivaraOS-HERD-1.0.0-x86_64.installer.iso` | Install on a physical server or VM from scratch |
| **qcow2 disk** | `CapivaraOS-HERD-1.0.0-x86_64.qcow2` | Spin up quickly on cloud/virtualization with `cloud-init` |

Download from:

- **Website:** [capivaraos.org — Download](https://capivaraos.org/en/download.html)
- **SourceForge:** [sourceforge.net/projects/capivaraos](https://sourceforge.net/projects/capivaraos/)

## Verify integrity

Always check the **SHA-256** of the downloaded file against the value published
next to the image (`.sha256` / `CHECKSUM` file).

=== "Linux / macOS"

    ```bash
    sha256sum CapivaraOS-HERD-1.0.0-x86_64.installer.iso
    # compare the output with the published value
    ```

=== "Windows (PowerShell)"

    ```powershell
    Get-FileHash .\CapivaraOS-HERD-1.0.0-x86_64.installer.iso -Algorithm SHA256
    ```

If a `CHECKSUM` file with the sums is provided, you can verify everything at once:

```bash
sha256sum -c CHECKSUM
```

!!! warning "If the checksum doesn't match"
    Do not use the image. A different hash means a corrupted download or a
    tampered file — download it again, preferably from the official source.

## Next step

- Physical server/VM → [Install via ISO](installation-iso.md)
- Cloud/virtualization → [Cloud/VM deployment](installation-cloud.md)
