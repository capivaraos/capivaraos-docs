# Herd by CapivaraOS

**Herd** 是 CapivaraOS 的**服务器**产品线：一个基于 **Fedora 44** 的 _无头_ Linux 系统（无图形桌面），旨在运行于物理服务器、虚拟机和云环境——带有巴西特色，默认具备合理的安全配置，并以 **Cockpit** 作为基于浏览器的管理控制台。

!!! info "社区版 1.0.1"
    本文档涵盖 **Herd Community**，即**免费且开源**的版本。架构为 **x86_64**（**aarch64** 支持将在 1.1 版本中推出）。

## 理念：herd = 集群

在 CapivaraOS 中，水豚（capybaras）群居在 **herd（兽群）** 中——这也是我们对服务器的看法：一个协同工作的 **fleet（集群）**。无论单节点还是多节点，Herd 都能提供一个可预测、已安全加固且易于管理的服务器基础。

## Herd Community 包含什么

| 领域 | 包含内容 |
|---|---|
| **基础系统** | Fedora 44（与 Marsh/Pup/Snout 1.x 桌面版属于同一代） |
| **版本定位** | _无头_ 服务器 —— 无桌面，最小化攻击面 |
| **Web 控制台** | **Cockpit**，位于 `https://<ip>:9090`（管理服务、网络、存储、账户、日志、终端） |
| **安全** | SELinux _enforcing_（强制模式）、严格的 `firewalld`、SSH 加固、密码策略、`umask 027` |
| **合规性** | `herd-compliance-scan` —— 内置 OpenSCAP 报告（SSG _standard_ 配置档） |
| **云** | qcow2 镜像上的 `cloud-init`（密钥/网络/用户注入） |
| **语言** | pt_BR 和 en_US；ABNT2 键盘；America/Sao_Paulo 时区（可调整） |

## 两种安装方式

<div class="grid cards" markdown>

-   :material-disc: **安装版 ISO（裸金属）**

    使用 Anaconda 文本安装程序，在物理服务器或虚拟机上全新安装。

    [:octicons-arrow-right-24: 通过 ISO 安装](installation-iso.md)

-   :material-cloud: **qcow2 镜像（云/虚拟机）**

    在云环境或虚拟化平台上快速启动，由 `cloud-init` 处理用户、SSH 密钥和网络配置。

    [:octicons-arrow-right-24: 云/虚拟机部署](installation-cloud.md)

</div>

## 从哪里开始

1. [系统要求](requirements.md) —— 服务器的硬件需求。
2. [下载与验证](download.md) —— 获取镜像并校验其完整性。
3. [通过 ISO 安装](installation-iso.md) **或** [云/虚拟机部署](installation-cloud.md)。
4. [快速入门](getting-started.md) —— 访问、网络、更新。
5. [Web 控制台 (Cockpit)](cockpit.md) —— 通过浏览器进行管理。
6. [安全与合规](security.md) —— 哪些内容已加固以及如何进行审计。

!!! tip "已经安装好了？"
    跳转到[快速入门](getting-started.md)或 [Web 控制台 (Cockpit)](cockpit.md)。

## 支持与社区

如有疑问、Bug 或建议：请参阅[支持](support.md)页面。Herd Community 由社区维护 —— 欢迎你的参与。

---

*内容在 AI 协助下生成，并处于持续审核中。发现错误？[提交 issue](https://github.com/capivaraos/capivaraos-herd/issues)。*