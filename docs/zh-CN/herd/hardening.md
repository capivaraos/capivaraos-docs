# 一键安全加固

Herd 默认出厂即带有[默认安全加固](security.md)。当你需要**超出基线**（例如满足 CIS 或 PCI-DSS 等特定标准）时，`herd-harden` 可以通过**一条命令**应用整个合规配置文件。

它是 [`herd-compliance-scan`](security.md#audit-herd-compliance-scan)（仅用于*报告*）的“执行”对应物：它使用相同的 Fedora **SCAP 安全指南 (SSG)** 内容，并通过 **Ansible** 应用修复。

!!! warning "默认情况下它不会改变任何东西"
    如果不加 `--apply` 参数，`herd-harden` 会运行一次**试运行**（Ansible 检查模式）：它会展示*将会*发生哪些更改，但不会触碰系统。只有明确指定 `--apply` 时才会真正应用更改。请始终先审查试运行结果。

## 可用的配置档 (Profiles)

| 配置档 | 目标 | 规则数 (约) | 说明 |
|---|---|---|---|
| `standard` | Herd 基线 (默认) | ~76 | 开箱即用（已默认应用） |
| `ospp` | 通用操作系统保护配置文件<br>(NIAP/通用准则) | ~208 | “官方”（非草案）配置档 |
| `cis-l1` | CIS Level 1 — Server | ~324 | 针对 Fedora 的 SSG 草案 |
| `cis-l2` | CIS Level 2 — Server | ~438 | 草案；限制更严格 |
| `pci` | PCI-DSS v3.2.1 | ~121 | 针对持卡人环境的控制项 |

你也可以传入**完整的技术配置文件 ID**（`xccdf_org.ssgproject.content_profile_*`）。使用 `herd-harden --list` 列出所有配置。

!!! info "对每个标准的预期 — 不做虚假承诺"
    - **STIG：** Fedora 没有 DISA STIG 配置文件（SSG 仅为 RHEL 提供该配置）。如果你的需求与 DoD 相关，**`ospp`** 是最接近的基线。
    - **CIS：** SSG 中的 Fedora CIS 配置文件是由社区维护的**草案**（CIS 不发布官方的 Fedora 基准）。它们实用且可靠，但请认清其性质。
    - **PCI-DSS：** 内置的配置文件版本为 **v3.2.1**。
    - Herd 提供**控制项和证据** —— 它使系统达到*合规就绪*状态。合规本身涉及流程、法律和组织，没有任何操作系统能代表你宣称自己“已认证”。

!!! warning "`ospp` 和 `cis` 会更改加密策略 —— 请注意 ed25519 密钥"
    `ospp` 和 `cis` 配置文件会启用 **FIPS** *加密策略*，该策略**拒绝 ed25519 SSH 密钥**（以及 FIPS 集合之外的密码套件）。在通过 SSH 访问的服务器上，请确保在应用前拥有 **RSA** 或 **ECDSA** 密钥，并在 `--apply` 之后立即**测试一次新的登录**，同时**不要关闭当前的会话** —— `herd-harden` 本身会在最后对此发出警告。详情请见[安全与合规](security.md#encryption-at-rest-and-fips-optional)页面。

## 用法

### 1. 查看将发生哪些更改（试运行）

```bash
sudo herd-harden cis-l1
```

这会生成该配置文件的修复剧本，并以**检查模式**（`--check --diff`）运行 Ansible：你将看到所有提议的更改，但不会应用任何更改。

### 2. 应用

```bash
sudo herd-harden cis-l1 --apply
```

更改将应用到此服务器。输出也会记录到日志中：

```
/var/log/herd-harden-<date>.log
```

### 3. 验证结果

使用扫描器在**相同的配置文件**上测量前后的变化：

```bash
sudo herd-compliance-scan cis-l1
```

### 其他命令

```bash
herd-harden --list      # 列出数据流配置文件
herd-harden --help      # 简短帮助
```

!!! tip "从最轻量的开始"
    一个不错的递进顺序是 `standard` → `ospp` → `cis-l1` → `pci`（或限制最严格的 `cis-l2`）。每次只应用一个，始终审查试运行结果并在之后测试你的服务 —— 更高级别的配置文件会对攻击面进行相当大程度的强化。

## 注意事项

- **先运行试运行。** 高级别的配置文件可能会限制你的应用程序所依赖的某些功能。在 `--apply` 之前请审查 `--diff` 的输出。
- **应用后进行测试。** 确认 SSH、你的服务和 Cockpit 是否正常。
- **`herd-harden` 需要 `ansible-core`**（Herd 已内置）。剧本基于内置的数据流生成 —— 支持离线工作。
- 你不希望被配置文件覆盖的特定设置应写入你自己的 drop-in 文件中（参见[调整](security.md#adjust)）。

## 第三方声明与许可

与 `herd-compliance-scan` 一样，`herd-harden` 使用 Fedora 的 **[SCAP 安全指南](https://github.com/ComplianceAsCode/content)**（`ssg-fedora-ds.xml`，BSD-3-Clause 许可，© 2012-2017 Red Hat, Inc.）。许可详情请见[安全与合规](security.md#third-party-credits-and-licenses)页面。配置文件名称来自 SSG 本身；“CIS”指的是[互联网安全中心 (Center for Internet Security)](https://www.cisecurity.org/)的基准 —— Herd 仅运行相应的配置文件，未复制基准文本。

---

*内容在 AI 协助下生成，并处于持续审核中。发现错误？[提交 issue](https://github.com/capivaraos/capivaraos-herd/issues)。*