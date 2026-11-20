# 通过 ISO 安装（裸金属/物理机）

本指南介绍如何使用安装版 ISO，在**物理**服务器或**虚拟机**上安装 Herd Community。安装程序采用**文本模式的 Anaconda** —— 非常适合 _无头_ 服务器，且不依赖图形界面。

!!! info "离线安装"
    ISO 镜像**自包含**了所需软件包，因此安装过程**不需要互联网连接**。

## 1. 准备启动介质

将 ISO 写入 USB 驱动器（≥ 2 GB）或将其作为虚拟介质挂载（iDRAC/iLO/IPMI，或虚拟机的“CD/DVD”驱动器）。

=== "Linux/macOS (dd)"

    ```bash
    lsblk   # 找到 USB 设备（例如 /dev/sdX）
    sudo dd if=CapivaraOS-HERD-1.0.1-x86_64.installer.iso of=/dev/sdX bs=4M status=progress oflag=sync
    ```

    !!! warning "确认设备"
        如果 `dd` 写错磁盘，将不可逆地擦除数据。请务必先用 `lsblk` 确认。

=== "Windows"

    使用 **Balena Etcher** 或 **Rufus**（DD/镜像模式）。

=== "虚拟机"

    将虚拟机的 CD/DVD 驱动器指向 `.iso` 文件即可。无需刻录 USB 驱动器。

## 2. 从 ISO 启动

进入服务器的启动菜单（通常是 **F11/F12/Del/Esc**）并选择该介质。出现 CapivaraOS GRUB 菜单 —— 选择 **Install Herd by CapivaraOS**。

Anaconda 将以**文本模式**启动并显示一个带编号的菜单。标记为 `[x]` 的项目已配置好；标记为 `[ ]` 的项目需要你进行设置。

## 3. 配置必需项

该配置文件已准备就绪（pt_BR + en_US 语言环境、ABNT2 键盘、America/Sao_Paulo 时区、DHCP 网络、离线安装源）。你需要手动设置：

### 磁盘（安装目标）

选择 **"Installation Destination"** 对应的数字，选择磁盘并确认自动分区（或自定义分区）。

!!! tip "分区"
    自动分区使用 LVM，适用于大多数情况。如果有特定的布局需求（例如单独的 `/var`、配额大小），请手动配置。

### Root 密码

在 **"Root password"** 下设置。默认情况下，**禁止**通过 SSH 直接使用 root 登录（参见[安全](security.md)）；root 密码用于**控制台**（物理机/IPMI）登录和系统恢复。

### 创建管理员用户

这一步很重要 —— 这是你日常使用的账户，也是登录 [Cockpit](cockpit.md) 使用的账户（Cockpit **禁止 root 登录**）。

1. 选择 **"User creation"** 对应的数字。
2. 输入 `1` 来**创建用户** —— 表单会展开。
3. 填写**用户名**和**密码**。
4. 勾选 **Administrator**（`[x]`） —— 这会将账户加入 `wheel` 组（允许使用 `sudo` 并在 Cockpit 中启用管理员权限）。
5. 输入 `c` 继续。

## 4. 开始安装

回到主菜单，当所有项都显示为 `[x]` 时，输入 `b`（_begin_）开始安装。安装完成后，**移除介质**并重启。

## 5. 首次启动

在 GRUB 菜单中，你将看到 **Herd** 条目。系统以文本模式启动，并在控制台打印出 Cockpit 地址：

```
Herd by CapivaraOS 1.0.1  ·  headless server (Fedora 44)
Web console: https://<ip>:9090/
```

## <a id="remote-access"></a>6. 远程访问 —— 必读

默认情况下，SSH 仅接受**密钥认证**（`PasswordAuthentication no`）。这意味着安装完成后，**你无法直接通过密码使用 SSH 登录**。可选方案：

- **首选控制台（推荐）：** 使用管理员用户在物理机/IPMI 控制台登录并添加你的公钥：
  ```bash
  install -d -m 700 ~/.ssh
  echo "ssh-ed25519 AAAA... you@machine" >> ~/.ssh/authorized_keys
  chmod 600 ~/.ssh/authorized_keys
  ```
  然后：`ssh user@<ip>`。
- **Cockpit：** `https://<ip>:9090` 的 Web 控制台接受**用户名 + 密码**（它不是 SSH）—— 你无需密钥即可进行管理。参见 [Web 控制台](cockpit.md)。
- **确实需要密码登录 SSH？** 你可以放宽策略，但需注意安全风险 —— 参见[安全 › 调整 SSH](security.md#ssh)。

## 下一步

- [快速入门](getting-started.md) —— 网络、主机名、更新。
- [Web 控制台 (Cockpit)](cockpit.md) —— 通过浏览器进行管理。
- [安全与合规](security.md) —— 默认进行了哪些加固。

---

*内容在 AI 协助下生成，并处于持续审核中。发现错误？[提交 issue](https://github.com/capivaraos/capivaraos-herd/issues)。*