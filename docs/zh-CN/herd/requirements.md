# 系统要求

Herd Community 1.0.1 面向 **x86_64**（64 位）架构发布。**aarch64**（ARM）支持将在 1.1 版本中推出。

## 最低和推荐硬件

| 组件 | 最低配置 | 推荐配置 |
|---|---|---|
| **架构** | x86_64（64 位） | x86_64 |
| **CPU** | 1 核 | 2 核或以上 |
| **内存** | 1 GB | 2 GB 或以上 |
| **磁盘** | 10 GB | 20 GB 或以上 |
| **网络** | 1 个以太网接口 | — |
| **固件** | Legacy BIOS **或** UEFI | UEFI |

!!! note "真实服务器与测试环境"
    上述最低配置足以启动并管理系统。对于生产工作负载（数据库、容器等），请根据你的应用程序规模来规划 CPU/内存/磁盘的容量。

## 固件与启动

- 安装版 ISO 同时支持 **UEFI** 和**Legacy BIOS**。
- 在启用了 **Secure Boot（安全启动）**的 UEFI 系统上，Herd 使用 Fedora 签名的 `shim` —— 在大多数硬件上无需禁用 Secure Boot 即可启动。

## 虚拟化与云

Herd 在虚拟化和云环境中运行良好。**qcow2** 镜像已自带 `qemu-guest-agent` 和 `cloud-init`：

| 平台 | 说明 |
|---|---|
| **KVM/QEMU, libvirt** | 直接使用 `qcow2` 镜像 |
| **Proxmox VE** | 将 `qcow2` 导入为虚拟机磁盘 |
| **VMware / VirtualBox** | 使用安装版 ISO，或转换 `qcow2` 镜像 |
| **云环境（OpenStack 等）** | `qcow2` + `cloud-init`（NoCloud/ConfigDrive 数据源） |

!!! tip "防火墙默认配置较为严格"
    默认情况下，`firewalld` 仅允许 **SSH (22)** 和 **Cockpit (9090)**。请规划好为你的应用程序开放所需端口 —— 参见[系统管理 › 防火墙](administration.md#firewall)。

## 网络

- 默认情况下，网络通过 **DHCP** 进行配置。
- 可以在安装过程（Anaconda）中设置静态 IP，安装后也可通过 [`nmcli`](getting-started.md#network) 或 [Cockpit](cockpit.md) 进行设置。
- 支持通过 DHCPv6 获取 IPv6 地址（`dhcpv6-client` 服务在防火墙中特意保持放行状态）。

---

*内容在 AI 协助下生成，并处于持续审核中。发现错误？[提交 issue](https://github.com/capivaraos/capivaraos-herd/issues)。*