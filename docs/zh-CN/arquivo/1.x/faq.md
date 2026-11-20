---
search:
  exclude: true
---

!!! warning "Series 1 — 已停止维护"
    本页记录的是 CapivaraOS 的 **Series 1**，它已经停止维护。当前版本是 **Series 2** — 参见[当前文档](/zh-CN/)。

# 常见问题

## CapivaraOS 是免费的吗？

是的，完全免费。这是个开源项目 — 下载、安装、使用都不花钱。

## Marsh、Pup 和 Snout 有什么区别？

区别在于桌面环境：

- **Marsh** 用 KDE Plasma — 功能齐全，高度可定制
- **Pup** 用 Xfce — 轻巧，配置不高的机器也合适
- **Snout** 用 GNOME — 干净，不折腾布局

三者的基础系统、视觉标识和必备应用都是一样的。

## CapivaraOS 会取代 Fedora 吗？

不算取代。CapivaraOS 是在 Fedora 之上构建的 — 它继承了 Fedora 的软件包系统、发布周期和硬件兼容性，但加上了巴西风格的视觉设计，以及开箱即用的葡萄牙语配置。

## 能在 CapivaraOS 上装 Fedora 的软件包吗？

能。它基于Fedora发行版，所以 Fedora 的软件仓库和软件包都能正常使用。包括DNF 和 Flatpak。

## CapivaraOS 支持 Flatpak 吗？

支持，Flatpak 已经预先配置好了。你可以正常从 Flathub 装应用。

## 更新多久发一次？

CapivaraOS 会跟随 Fedora 的发布周期进行更新。安全和软件包更新会持续推送，新的主要 ISO 版本则定期发布。

## 去哪儿反馈问题？

去论坛，或者在 GitHub 上开一个 issue。参见[如何贡献](../../contribute.md)页面。

## CapivaraOS 能在虚拟机里跑吗？

能，VirtualBox、VMware 和 QEMU/KVM 都没问题。用 Marsh 和 Snout 的话，建议打开 3D 加速。

## 能把 CapivaraOS 装成双系统吗？

能。Anaconda 安装器支持手动分区，可以和 Windows 或其他 Linux 发行版组双系统。
