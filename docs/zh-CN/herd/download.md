# 下载与验证

## 选择镜像

Herd Community 1.0.1 提供两种 **x86_64** 架构的镜像：

| 镜像 | 文件 | 适用场景 |
|---|---|---|
| **安装版 ISO** | `CapivaraOS-HERD-1.0.1-x86_64.installer.iso` | 在物理服务器或虚拟机上全新安装 |
| **qcow2 磁盘镜像** | `CapivaraOS-HERD-1.0.1-x86_64.qcow2` | 配合 `cloud-init`，在云环境/虚拟化平台上快速启动 |

下载地址：

- **官方网站：** [capivaraos.org — 下载页面](https://capivaraos.org/en/download.html)
- **SourceForge：** [CapivaraOS-HERD/1.0.1 文件夹](https://sourceforge.net/projects/capivaraos/files/CapivaraOS-HERD/1.0.1/)

## 校验完整性

请务必将下载文件的 **SHA-256** 校验和与公布的值进行核对。
这两个镜像的校验和均包含在下载目录下的 `SHA256SUMS` 文件中：

```
395d06472e47b9b572769fe35f295806da01abb6536a89e5b5b82a0ec757776b  CapivaraOS-HERD-1.0.1-x86_64.qcow2
3a38b869c4f8b883e35de2fe2126d2f90b84b244f4c81481e67414e78a8d0d71  CapivaraOS-HERD-1.0.1-x86_64.installer.iso
```

=== "Linux / macOS"

    ```bash
    # 将 SHA256SUMS 下载到同一文件夹，然后运行：
    sha256sum -c SHA256SUMS
    # 对于你下载的文件，它应该输出 "OK"
    ```

=== "Windows (PowerShell)"

    ```powershell
    Get-FileHash .\CapivaraOS-HERD-1.0.1-x86_64.installer.iso -Algorithm SHA256
    # 将输出结果与上面的值进行对比
    ```

!!! warning "如果校验和不匹配"
    请勿使用该镜像。哈希值不一致意味着下载损坏或文件被篡改 —— 请重新下载，最好从官方来源获取。

!!! tip "校验和不能证明文件的来源"
    它只能证明下载过程完整无损。要确认该软件确实由我们发布，请检查 GPG 签名 —— [验证软件来源](security.md#verify-the-software-is-ours)。

## 下一步

- 物理服务器/虚拟机 → [通过 ISO 安装](installation-iso.md)
- 云环境/虚拟化 → [云/VM 部署](installation-cloud.md)

---

*内容在 AI 协助下生成，并处于持续审核中。发现错误？[提交 issue](https://github.com/capivaraos/capivaraos-herd/issues)。*