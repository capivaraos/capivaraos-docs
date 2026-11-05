# Verify your download

Before you write the USB stick, it's worth checking the file you downloaded.
There are two different questions, each with its own answer:

- **Did the file arrive intact?** → the **SHA-256 checksum**.
- **Is the file really ours?** → the **GPG signature**.

The first catches a corrupted or half-finished download (the most common reason
an ISO won't boot). The second proves the image was published by CapivaraOS and
not swapped by someone else. The second step is optional, but it's quick and
recommended.

## What to download

In each edition's folder ([capivaraos.org/en/download.html](https://capivaraos.org/en/download.html)
or [SourceForge](https://sourceforge.net/projects/capivaraos/files/)), next to
the ISO you'll find:

| File | What it's for |
|---|---|
| `CapivaraOS-<Edition>-2.0.0-x86_64.iso` | the image |
| `...iso.sha256` | the published checksum |
| `...iso.sha256.asc` | the GPG signature of the checksum |
| `...iso.torrent` | BitTorrent download (verifies itself as it goes) |

!!! tip "The torrent already verifies for you"
    If you download via the `.torrent`, your BitTorrent client checks every piece
    against its hash while downloading — a torrent download that finishes is
    already intact. The GPG signature (below) is still worth doing.

## 1. Did the file arrive intact? (checksum)

Download the `.sha256` into the **same folder** as the ISO and run:

=== "Linux / macOS"

    ```bash
    sha256sum -c CapivaraOS-Snout-2.0.0-x86_64.iso.sha256
    # should answer: CapivaraOS-Snout-2.0.0-x86_64.iso: OK
    ```

    (on macOS use `shasum -a 256 -c` if you don't have `sha256sum`.)

=== "Windows (PowerShell)"

    ```powershell
    Get-FileHash .\CapivaraOS-Snout-2.0.0-x86_64.iso -Algorithm SHA256
    # compare the output with your edition's value in the table below
    ```

The published **2.0.0** values are:

```
Marsh   79666af3bee34b6ad9477c3e4f49aee5e0d621931b80163bb88e30dc2b03dd22
Pup     13acef6ce12cb0120f5cdb8890ef2d3a5f7607e324516611afe4cfb4a481209f
Snout   8e25df502c4a22b6ac898fde9347aa03c47a28220a2bcdd83dc1981e5e212bd1
```

!!! warning "If the checksum doesn't match"
    Don't use the image and don't write the stick — a different hash means a
    corrupted or tampered file. Download it again, preferably via the
    **torrent**, which verifies itself.

## 2. Is the file really ours? (GPG signature)

The checksum proves the file arrived intact, but not **who** published it:
anyone who swapped the image would publish a matching checksum too. The signature
answers that. The `.sha256.asc` is the GPG signature of the checksum file, made
with our key. Its private half was **generated in an offline ceremony** and is
**kept in a vault**: it enters a machine only at the moment of signing a release,
and is removed afterwards.

**a) Download and import our public key:**

```bash
curl -O https://repo.capivaraos.org/RPM-GPG-KEY-capivaraos
gpg --import RPM-GPG-KEY-capivaraos
```

**b) Check the fingerprint.** This is the step that matters — it must be
**exactly**:

```
75FA 3340 40FD 0150 4AAD  9337 E07F 78EA 4070 11BD
```

```bash
gpg --show-keys --with-fingerprint RPM-GPG-KEY-capivaraos
# uid: CapivaraOS (repo signing key) ... <capivaraos-bot@users.noreply.github.com>
```

!!! danger "If the fingerprint doesn't match"
    Stop. A different key means the file is signed by someone who isn't us.
    Don't write the image and report it through the
    [security channel](https://www.capivaraos.org/en/trust-center.html).

**c) Verify the signature on the checksum** (both files in the same folder):

```bash
gpg --verify CapivaraOS-Snout-2.0.0-x86_64.iso.sha256.asc \
             CapivaraOS-Snout-2.0.0-x86_64.iso.sha256
# expected: Good signature from "CapivaraOS (repo signing key) ..."
```

The `WARNING: This key is not certified with a trusted signature` message is
normal: it only means you haven't signed our key in your own *web of trust*. What
matters is the **Good signature** and the fingerprint you checked in step (b).

!!! note "The same key signs your updates"
    It's the same key that signs the installed system's
    [update repository](../update/index.md). Check the fingerprint once and you
    recognize both the ISO and the updates.

## Next step

File intact and signature verified? Head to the
[**Installation guide**](guide.md) to write the stick and install.
