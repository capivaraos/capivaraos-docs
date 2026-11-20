# Pup

**桌面环境：** Xfce 4.20 · **基于：** Fedora 45 · **架构：** x86_64

Pup 是轻量版。Xfce 内存占用低，在老机器上运行流畅，但在这里它并非精简版：Pup 预装了与其他两个版本**相同的开发工具集**。

它是配置一般的硬件的首选，也适合任何单纯偏好更直接桌面体验的用户。

## 预装内容

- **Xfce 4.20**
- **Firefox** 默认浏览器
- **LibreOffice**（含葡萄牙语翻译）
- **VLC** 音视频播放器
- **Snapper**（用于 Btrfs 快照）
- CapivaraOS 专属的 **COS Dev Center** 和 **COS Config Center**

Pup 有意保持精简：默认不包含重型应用，你可以按需安装。

## 面向开发者

[COS Dev Center](../apps/cos-dev.md) 会在首次启动时打开，提供与其他版本相同的功能 —— 本地 AI、语言环境、开发工具和 GitHub 配置。

!!! note "在配置一般的机器上使用本地 AI"
    本地 AI 需要下载好几 GB 的数据，如果没有独立显卡，它将在 CPU 上运行。在老电脑上虽然能跑，但速度很慢：建议优先选择较小的模型，或者跳过此项。

## 视觉风格

与其他版本采用相同的视觉风格 —— Plymouth、GRUB、图标和主题 —— 并附带我们自有的壁纸集，其中包括一款 Pup 专属壁纸。

## 升级

从 Series 2 开始，Pup 升级**无需重装**。请参阅[更新系统](../update/index.md)。

!!! warning "在 Pup 上，请通过终端升级"
    Xfce 的默认更新工具 `dnfdragora` 在 Fedora 45 上**不会列出 CapivaraOS 的更新**。这不是我们软件仓库的问题 —— 而是 `dnfdaemon` 签名检查失败导致的。在修复之前，请在终端中使用 `sudo dnf upgrade`。详情请参阅[更新系统](../update/index.md)。

## 系统要求

请参阅[硬件要求](../installation/requirements.md)。Pup 是三个版本中对硬件要求最低的。

## 下载

[下载 Pup](https://capivaraos.org/en/download.html){ .md-button .md-button--primary }

写入前请校验 ISO：[验证你的下载](../installation/verify-download.md)。