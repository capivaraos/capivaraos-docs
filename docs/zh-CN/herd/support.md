# 支持与生命周期

## 版本

**Herd Community** 是**免费且开源**的（代码基于 GPLv3 许可；名称和徽标是项目的商标）。它由社区维护，基于**尽最大努力**（best-effort）的原则 —— 不提供具有合同约束力的 SLA（服务级别协议）。

## 生命周期

- Herd **1.x** 基于 **Fedora 44**，并跟随该基础系统的安全更新。请使用 `dnf upgrade` 保持系统更新。
- **第 2 代**（Herd 2.x）将迁移到更新的 Fedora 基础系统，并在其成熟时发布。

!!! tip "保持更新"
    最好的安全防护就是保持系统最新。请定期运行 `dnf upgrade`（或使用 [Cockpit](cockpit.md) 中的**软件更新**模块）。

## 报告 Bug

发现问题了？请在 GitHub 上提交 issue：

- **[github.com/capivaraos/capivaraos-herd/issues](https://github.com/capivaraos/capivaraos-herd/issues)**

提供以下信息将大有帮助：

- 版本信息 (`cat /etc/os-release`)；
- 如何复现；
- 相关日志 (`journalctl -b -p err`，或受影响服务的日志)；
- 如果适用，提供 `herd-compliance-scan` 的输出结果。

## 社区

- **网站：** [capivaraos.org](https://capivaraos.org)
- **新闻：** [capivaraos.org/en/blog.html](https://capivaraos.org/en/blog.html)
- **社交账号：** Instagram、X 和 YouTube (`@CapivaraOS` / `@Capivara-OS`)

## 贡献

CapivaraOS 是一个开放的巴西项目。Bug 报告、文档改进、测试和宣传都非常欢迎 —— 参见[贡献](../contribute.md)页面。

## 常见问题 (FAQ)

??? question "安装完成后，我可以直接通过 SSH 使用密码登录吗？"
    不可以 —— SSH 是**仅限密钥认证**的（`PasswordAuthentication no`）。请通过控制台/Cockpit 添加你的密钥，或者（在注意风险的前提下）根据[安全 › SSH](security.md#ssh) 启用密码认证。

??? question "Cockpit 接受 root 登录吗？"
    不接受。请使用管理员用户（`wheel` 组）并点击**开启管理员权限**。

??? question "有 ARM (aarch64) 版本吗？"
    1.0.1 版本还没有 —— aarch64 支持将在 **1.1** 版本中推出。

??? question "我可以在商业生产环境中使用它吗？"
    可以。该软件是自由的（GPLv3），可用于任何目的，包括商业用途。仅**商标**（名称/徽标）受保护 —— 请参阅仓库中的 `TRADEMARK.md`。

??? question "如何搭建 Web 服务器/数据库/容器？"
    Herd 是一个精简的服务器基础系统。使用 `dnf` 安装你需要的软件（`nginx`、`podman`、`postgresql-server` 等），并在[防火墙](administration.md#firewall)中开放相应端口。

---

*内容在 AI 协助下生成，并处于持续审核中。发现错误？[提交 issue](https://github.com/capivaraos/capivaraos-herd/issues)。*