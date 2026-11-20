# 安装指南

## 1. 下载 ISO

前往[下载页面](https://capivaraos.org/en/download.html)并选择你的发行版：[Marsh](../distros/marsh.md)、[Pup](../distros/pup.md) 或 [Snout](../distros/snout.md)。

## 2. 校验下载的文件

在写入之前，请校验 ISO 的 SHA-256。这很快，并且能排除 U 盘无法启动的最常见原因：下载文件损坏。

如果你想确保文件不仅完整无缺，而且确实**属于我们**，请同时校验签名。具体步骤见[验证你的下载](verify-download.md)。

## 3. 写入 USB 驱动器

你需要一个至少 4 GB 的 USB 驱动器。我们推荐使用 **Fedora Media Writer** 或 **balenaEtcher** —— 它们能将镜像直接写入驱动器，开箱即用：

- **Fedora Media Writer**（Windows、macOS、Linux）—— Fedora 官方工具，最佳选择
- **balenaEtcher**（Windows、macOS、Linux）—— 简单，只需点击三次
- **dd**（Linux/macOS，通过终端）：

```bash
sudo dd if=capivaraos-marsh-2.0.0.iso of=/dev/sdX bs=4M status=progress
```

!!! warning "使用 dd 时请小心"
    运行命令前，请使用 `lsblk` 确认正确的设备名称。覆盖错误的设备是不可逆的。

!!! note "如果 Ventoy 在启动时卡住"
    Ventoy 允许在一个 U 盘中存放多个 ISO 镜像，在大多数情况下都能正常工作，但在某些电脑上，Live 会话无法启动：引导过程停留在 *"Starting CapivaraOS…"*，有时会出现类似 `Can't create /lib/dracut/hooks/.../99-ventoy-….sh` 的错误信息。Ventoy 通过 *loopback（回环设备）* 加载 ISO，并将该脚本注入到引导过程中 —— 这是 Ventoy 的错误，而不是镜像的问题。出现这种情况时，请使用 Fedora Media Writer、balenaEtcher 或 `dd` **将 ISO 直接写入 U 盘**，然后重新启动。

## 4. 从 USB 启动

重启电脑并进入启动菜单（通常在启动过程中按 F12、F2 或 Del 键）。选择 USB 驱动器。

在菜单中，你将看到 **CapivaraOS \<edition\> 2 45** —— 其中 2 是 CapivaraOS 的版本号，45 是它所基于的 Fedora 版本。

## 5. 体验或安装

启动后，你将看到进入 Live 模式（不安装直接体验）或直接开始安装的选项。

!!! tip "如果可以，请先体验"
    Live 模式在内存中运行，不会触碰你的磁盘。这是在安装任何内容之前，确认你的机器无线网络、声音和视频是否正常工作的好方法。

## 6. 按照安装程序的指引操作

CapivaraOS 使用与 Fedora 相同的 Anaconda 安装程序。它将引导你完成：

1. **语言** —— 系统预置了巴西葡萄牙语和英语；你可以在安装过程中选择，之后也可以在 [COS Config Center](../apps/cos-config.md) 中更改
2. **磁盘分区** —— 默认使用 Btrfs，这是能够在每次升级前创建快照的基础
3. **创建用户账户**
4. **安装**

!!! note "加密磁盘"
    如果你选择加密（LUKS），电脑在每次启动时都会在一个带有水豚图案的屏幕上要求输入磁盘密码。请牢记该密码：没有它，磁盘将无法打开。

## 7. 首次启动

拔下 USB 驱动器并重启。首次登录时，**[COS Dev Center](../apps/cos-dev.md)** 会自动打开，并询问你要安装哪些内容 —— 本地 AI、语言环境、开发工具。这些都不是强制性的：你可以关闭它，稍后从菜单中再次打开。

此后，系统将保持更新而无需重装。请参阅[保持系统更新](../update/index.md)。

---

遇到问题？请在[论坛](https://capivaraos.org)发帖或在 GitHub 上提交 issue。