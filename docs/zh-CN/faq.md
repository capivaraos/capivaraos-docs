# 常见问题 (FAQ)

## CapivaraOS 是免费的吗？

是的，完全免费。这是一个开源项目 —— 可以自由下载、安装和使用。

## Marsh、Pup 和 Snout 之间有什么区别？

区别在于桌面环境：

- **Marsh** 使用 KDE Plasma —— 功能齐全且高度可定制
- **Pup** 使用 Xfce —— 轻量级，非常适合配置一般的硬件
- **Snout** 使用 GNOME —— 简洁，未对布局进行调整

基础系统、视觉风格、两个专属应用（[COS Dev Center](apps/cos-dev.md) 和 [COS Config Center](apps/cos-config.md)）以及开发工具集在三个版本中完全相同。

## CapivaraOS 会取代 Fedora 吗？

不完全是。CapivaraOS 构建于 Fedora 之上 —— 它继承了 Fedora 的软件包系统、发布周期和硬件兼容性，但增加了自己的视觉风格、两个专属应用以及首次启动即可用的开发环境。

## 我可以在 CapivaraOS 上安装 Fedora 软件包吗？

可以。由于基础系统是 Fedora，所有 Fedora 软件仓库和软件包均可正常工作。DNF 和 Flatpak 都可以使用。

## CapivaraOS 支持 Flatpak 吗？

支持，Flatpak 已预配置。你可以像平常一样从 Flathub 安装应用。

## 更新多久发布一次？

CapivaraOS 遵循 Fedora 的发布周期。安全和软件包更新会持续推送，可通过软件中心或 `dnf` 获取。

## 我必须重装系统才能获取下一个版本吗？

不需要，从 Series 2 开始就不用了。系统可以在不同版本和代际之间**原地**迁移，无需重新格式化。在桌面版中（安装在 Btrfs 上），系统会首先创建快照，以便你可以回退。参见[保持系统更新](update/index.md) —— 对于服务器，请参见[保持 Herd 更新](herd/update.md)，其默认布局没有自动快照，需要通过备份进行恢复。

Series 1 是个例外：它没有这条升级路径，需要全新安装 —— 这是最后一次需要重装。

## 我如何知道我下载的 ISO 真的是你们的？

每个 ISO 都附带其 SHA-256 校验和，以及使用我们密钥对该校验和生成的签名。校验和证明了文件下载完整；签名证明了是谁发布的它。具体步骤见[验证你的下载](installation/verify-download.md)。

## 本地 AI 会将我的数据发送到任何地方吗？

不会。模型在你的机器上运行，无需云端，也无需账户。你的数据不会流向任何外部网络，除了初次下载模型。 详情见 [COS Dev Center](apps/cos-dev.md)。

## 我在哪里报告问题？

在论坛上，或在 GitHub 上提交 issue。参见[贡献](contribute.md)页面。

## CapivaraOS 可以在虚拟机中运行吗？

可以，它可以在 VirtualBox、VMware 和 QEMU/KVM 中运行。对于 Marsh 和 Snout，我们建议启用 3D 加速。

在虚拟机中，本地 AI 运行在 CPU 上 —— 它可以工作，但速度较慢。

## 我可以在双系统（双启动）中使用 CapivaraOS 吗？

可以。Anaconda 安装程序支持手动分区，以便与 Windows 或其他 Linux 发行版双启动。