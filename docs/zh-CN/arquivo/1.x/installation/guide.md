---
search:
  exclude: true
---

!!! warning "Series 1 — 生命周期结束 (EOL)"
    本页面记录的是 CapivaraOS 的 **Series 1** 版本，该版本已达到生命周期终点。当前版本为 **Series 2** —— 请参阅[当前文档](/en/)。

# 安装指南

## 1. 下载 ISO 镜像

前往[下载页面](https://capivaraos.org/en/download.html)并选择你的发行版：Marsh、Pup 或 Snout。

## 2. 写入 USB 驱动器

你需要一个至少 4 GB 的 USB 驱动器。使用以下工具之一：

- **Balena Etcher**（Windows、macOS、Linux）—— 最简单的选择
- **Ventoy**（Windows、Linux）—— 允许你在同一个驱动器上存放多个 ISO 镜像
- **dd**（Linux/macOS，通过终端）：

```bash
sudo dd if=capivaraos-marsh-1.x.x.iso of=/dev/sdX bs=4M status=progress
```

!!! warning "使用 dd 命令时请谨慎操作"
    运行命令前，请使用 `lsblk` 确认正确的设备名称。覆盖错误的设备是不可逆的。

## 3. 从 USB 启动

重启电脑并进入启动菜单（通常在启动过程中按 F12、F2 或 Del 键）。选择 USB 驱动器。

## 4. 体验或安装

启动后，你将看到进入 Live 模式（不安装直接体验）或直接开始安装的选项。

## 5. 跟随 Anaconda 安装程序

CapivaraOS 使用与 Fedora 相同的 Anaconda 安装程序。它将引导你完成：

1. 语言选择（默认预配置为巴西葡萄牙语）
2. 磁盘分区
3. 创建用户账户
4. 安装

## 6. 完成

安装完成后，拔下 USB 驱动器并重启。系统已预配置为葡萄牙语，桌面环境已准备就绪，可直接使用。

---

遇到问题？请在[论坛](https://capivaraos.org)发帖或在 GitHub 上提交 issue。