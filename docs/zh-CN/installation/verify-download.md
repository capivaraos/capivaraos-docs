# 验证你的下载

在将镜像写入 USB 驱动器之前，值得检查一下你下载的文件。这里有两个不同的问题，每个都有对应的答案：

- **文件下载完整了吗？** → 检查 **SHA-256 校验和**。
- **文件真的是我们的吗？** → 检查 **GPG 签名**。

第一步能抓出损坏或未下载完整的文件（这是 ISO 无法启动的最常见原因）。第二步则能证明该镜像是 CapivaraOS 发布的，而不是被别人掉包过的。第二步是可选的，但它很快，且强烈推荐。

## 需要下载什么

在每个版本的文件夹中（[capivaraos.org/en/download.html](https://capivaraos.org/en/download.html) 或 [SourceForge](https://sourceforge.net/projects/capivaraos/files/)），你会看到 ISO 旁边有以下文件：

| 文件 | 用途 |
|---|---|
| `CapivaraOS-<Edition>-2.0.0-x86_64.iso` | 镜像文件本体 |
| `...iso.sha256` | 公布的校验和 |
| `...iso.sha256.asc` | 校验和文件的 GPG 签名 |
| `...iso.torrent` | BitTorrent 下载文件（边下载边自我校验） |

!!! tip "Torrent 下载会自动为你校验"
    如果你通过 `.torrent` 下载，你的 BitTorrent 客户端会在下载时根据哈希值校验每一个数据块 —— 下载完成的种子文件本身就是完整的。不过，下面提到的 GPG 签名仍然值得一做。

## 1. 文件下载完整了吗？（校验和）

将 `.sha256` 文件下载到与 ISO **相同的文件夹**中，并运行：

=== "Linux / macOS"

    ```bash
    sha256sum -c CapivaraOS-Snout-2.0.0-x86_64.iso.sha256
    # 应该会输出：CapivaraOS-Snout-2.0.0-x86_64.iso: OK
    ```

    （如果你没有 `sha256sum` 命令，在 macOS 上可以使用 `shasum -a 256 -c`。）

=== "Windows (PowerShell)"

    ```powershell
    Get-FileHash .\CapivaraOS-Snout-2.0.0-x86_64.iso -Algorithm SHA256
    # 将输出结果与下表中所选版本的值进行比对
    ```

公布的 **2.0.0** 校验和如下：

```
Marsh   79666af3bee34b6ad9477c3e4f49aee5e0d621931b80163bb88e30dc2b03dd22
Pup     13acef6ce12cb0120f5cdb8890ef2d3a5f7607e324516611afe4cfb4a481209f
Snout   8e25df502c4a22b6ac898fde9347aa03c47a28220a2bcdd83dc1981e5e212bd1
```

!!! warning "如果校验和不匹配"
    不要使用该镜像，也不要将其写入 U 盘 —— 哈希值不同意味着文件损坏或已被篡改。请重新下载，最好通过能自我校验的 **torrent** 进行下载。

## 2. 文件真的是我们的吗？（GPG 签名）

校验和证明了文件下载完整，但无法证明**是谁**发布的：任何替换了镜像的人也可以同时发布一个匹配的假校验和。签名解决了这个问题。`.sha256.asc` 是校验和文件的 GPG 签名，由我们的密钥生成。其私钥部分是在**离线仪式**中生成的，并**保存在保险库中**：它仅在签署版本的那一刻进入机器，随后立即被移除。

**a) 下载并导入我们的公钥：**

```bash
curl -O https://repo.capivaraos.org/RPM-GPG-KEY-capivaraos
gpg --import RPM-GPG-KEY-capivaraos
```

**b) 检查指纹。** 这是最重要的步骤 —— 指纹**必须完全一致**：

```
75FA 3340 40FD 0150 4AAD  9337 E07F 78EA 4070 11BD
```

```bash
gpg --show-keys --with-fingerprint RPM-GPG-KEY-capivaraos
# uid: CapivaraOS (repo signing key) ... <capivaraos-bot@users.noreply.github.com>
```

!!! danger "如果指纹不匹配"
    停止操作。不同的密钥意味着该文件是由并非我们的人签名的。不要写入该镜像，并通过[安全渠道](https://www.capivaraos.org/en/trust-center.html)报告此事。

**c) 验证校验和文件的签名**（确保两个文件在同一个文件夹中）：

```bash
gpg --verify CapivaraOS-Snout-2.0.0-x86_64.iso.sha256.asc \
             CapivaraOS-Snout-2.0.0-x86_64.iso.sha256
# 预期输出：Good signature from "CapivaraOS (repo signing key) ..."
```

出现 `WARNING: This key is not certified with a trusted signature` 警告信息是正常的：这仅仅意味着你还没有在我们的密钥上签署你自己的*信任网 (web of trust)*。真正重要的是看到 **Good signature** 以及你在步骤 (b) 中检查过的指纹。

!!! note "签名系统更新的也是同一把密钥"
    它与为已安装系统签署[更新软件仓库](../update/index.md)的密钥是同一把。检查一次指纹，你就可以同时确认 ISO 镜像和后续系统更新的真实性。

## 下一步

文件完整且签名验证通过？前往[**安装指南**](guide.md)写入 U 盘并开始安装。