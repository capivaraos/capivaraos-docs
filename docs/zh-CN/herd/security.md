# 安全与合规

Herd **默认已进行安全加固** —— 全新安装的服务器自带合理的安全配置档，你无需进行任何配置。本页面解释了**启用了哪些功能**、如何**审计**以及如何**调整**。

## 加固了哪些内容

| 领域 | Herd 默认配置 |
|---|---|
| **SELinux** | `enforcing`（强制模式） |
| **防火墙** | `firewalld` 仅允许 **SSH** 和 **Cockpit**（以及 `dhcpv6-client`）；关闭 `mdns` |
| **SSH** | 仅限密钥；禁止 root 登录；限制登录尝试次数（见下文） |
| **密码** | 最低质量要求（`pwquality`）和密码老化策略 |
| **umask** | 登录会话设为 `027`（新文件不可被全局读取） |
| **软件包** | `gpgcheck=1`（在 dnf 中验证签名） |

### SSH —— 默认策略

文件：`/etc/ssh/sshd_config.d/50-capivaraos-herd.conf`

```text
PermitRootLogin no
PasswordAuthentication no
PermitEmptyPasswords no
KbdInteractiveAuthentication no
X11Forwarding no
MaxAuthTries 3
LoginGraceTime 30
AllowAgentForwarding no
```

### 密码策略

- 密码强度 —— `/etc/security/pwquality.conf.d/50-capivaraos-herd.conf`：最小长度 **12**，至少包含 **3 种字符类别**，不允许长重复/序列。
- 密码有效期 —— `/etc/login.defs`：最长 **90** 天，最短 **7** 天。

!!! note "加密套件（加密策略）"
    SSH 和 TLS 的加密套件/MAC/KEX 遵循**系统加密策略**（Fedora 默认已足够强），而非在 Herd 中硬编码的值。

## 审计：`herd-compliance-scan`

Herd 内置了 **OpenSCAP** 扫描器以及 Fedora 的 **SCAP 安全指南 (SSG)** 内容。
运行：

```bash
sudo herd-compliance-scan
```

这会将系统与 **`standard`** 配置档进行比对评估，并生成以下文件：

```
/root/herd-compliance-<date>.html      # 可读报告（用浏览器打开）
/root/herd-compliance-<date>.xml       # XCCDF 结果（供机器读取）
/root/herd-compliance-<date>.arf.xml   # ARF：可重新渲染的证据/审计文件
```

要列出可用的配置档或运行其他配置档（它接受与 [`herd-harden`](hardening.md) **相同的别名** —— `standard`、`ospp`、`cis-l1`、`cis-l2`、`pci` —— 或完整的 ID）：

```bash
herd-compliance-scan --list
sudo herd-compliance-scan ospp
```

!!! tip "ARF 就是证据"
    `.arf.xml` 是*机器可读*的格式，它将数据流和结果打包在一起：你可以**在稍后无需重新扫描的情况下重新生成报告**，这也是审计/聚合工具所使用的格式。

!!! tip "将报告复制到你的电脑"
    ```bash
    scp user@<ip>:/root/herd-compliance-*.html .
    ```
    （或者通过 Cockpit 的 **Terminal**/**Files** 模块打开文件。）

!!! info "为什么 `standard` 是默认值"
    `standard` 配置档是一个平衡的基线：它关闭了重要的风险（SSH、防火墙、SELinux、密码、umask），而不会破坏普通服务。这就是它默认运行的原因。

    更严格的配置档 —— `cis-l1`、`cis-l2`、`ospp` 和 `pci` —— **随系统一起提供，只需一条命令即可运行**，无需许可证，也无需安装任何东西：

    ```bash
    sudo herd-harden cis-l2        # 默认试运行
    sudo herd-harden cis-l2 --apply
    ```

    它们会改变非常多的内容（CIS Level 2 会评估 438 条规则，而 `standard` 只有 76 条），并且可能会破坏你依赖的服务。请先运行试运行，阅读将要更改的内容，然后再做决定 —— 这由你决定，而不是版本限制。

## 验证软件来源

镜像的校验和证明了文件在传输过程中完好无损 —— 参见[下载与验证](download.md)。但它**不能**证明是谁制作了它：任何替换了镜像的人都可以同时发布新的校验和。GPG 签名才是回答这个问题的关键。

这一切都不需要你信任我们。以下是你自己可以运行的四个检查。

### 1. 密钥是否已安装？

公钥随系统一起提供，并由软件仓库的 RPM 包导入：

```bash
rpm -q gpg-pubkey --qf '%{SUMMARY}\n' | grep -i capivaraos
```

### 2. 指纹是否匹配？

这是最重要的步骤。请将你机器上的值与这里公布的值进行比对：

```bash
gpg --show-keys --with-fingerprint /etc/pki/rpm-gpg/RPM-GPG-KEY-capivaraos-herd
```

指纹**必须完全一致**：

```
C852 6722 0A81 18C9 864F  0648 BE5B 82AD C8A2 85C0
```

!!! danger "如果不匹配"
    停止操作。不同的密钥意味着系统信任了并非我们的人。不要应用更新，并通过[私密安全渠道](https://www.capivaraos.org/en/trust-center.html)报告此事。

Herd 的密钥与桌面版使用的密钥是**分开的**（`75FA 3340 40FD 0150 4AAD  9337 E07F 78EA 4070 11BD`）。其中一个被攻破不会影响另一个。

### 3. 安装的软件包是否已签名？

```bash
# 在安装前验证下载的 RPM 的签名
rpm -K ./some-package.rpm
# 预期输出：...: digests signatures OK

# 查看已安装软件包的签名者
rpm -qi capivaraos-herd-hardening | grep -i signature
```

未签名的软件包，或由其他密钥签名的软件包，会显示为 `NOT OK`、`MISSING KEYS` 或 `(none)`。

### 4. 软件仓库是否强制要求签名？

```bash
grep -E 'gpgcheck|repo_gpgcheck' /etc/yum.repos.d/capivaraos-herd.repo
```

两者都必须为 `1`。`gpgcheck` 验证每个软件包；`repo_gpgcheck` 还会验证仓库的**元数据** —— 如果没有这一项，控制服务器的人就可以发布不同的软件包列表，即使他们无法对软件包本身进行签名。

!!! warning "`makecache` 不是测试标准"
    在 dnf5 上，当元数据签名不正确时，`dnf makecache` 只会**发出警告**，并且仍然返回 0。**事务**才是拒绝它的地方。要进行真正的测试，请尝试实际的操作（`dnf check-update` 或 `dnf update`）—— 那才是被篡改的仓库被拒绝的地方。

### 为什么即使服务器沦陷了这也依然成立

**私钥**从未出现在仓库服务器上。它在一个离线仪式中生成并保存在保险库中；它仅在签署版本的那一刻进入机器，随后立即被移除。因此，即使服务器被攻破，它最多也只能提供你的系统会拒绝的文件，因为无法对它们进行签名。这就是为什么信任建立在密钥上，而不是托管服务器的人身上。

## 调整

所有的配置都来自 `capivaraos-herd-hardening` 包。你可以覆盖它来匹配你的策略。

### <a id="ssh"></a>SSH —— 启用密码登录（附警告）

!!! warning "三思而后行"
    启用 SSH 密码认证会增加攻击面（暴力破解）。首选**密钥认证**。仅在确实需要时才启用密码认证，并将其与 `fail2ban`/防火墙来源限制结合使用。

创建一个优先级**排在**我们配置之后的 _Drop-in 配置文件_（按字母顺序）：

```bash
sudo tee /etc/ssh/sshd_config.d/60-local.conf >/dev/null <<'EOF'
PasswordAuthentication yes
EOF
sudo systemctl restart sshd
```

### 调整密码规则

```bash
sudo tee /etc/security/pwquality.conf.d/60-local.conf >/dev/null <<'EOF'
minlen = 14
EOF
# 密码有效期策略：编辑 /etc/login.defs (PASS_MAX_DAYS / PASS_MIN_DAYS)
```

### 添加你的 SSH 密钥

```bash
install -d -m 700 ~/.ssh
echo "ssh-ed25519 AAAA... you@machine" >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```

!!! warning "打算应用 `ospp`/`cis` 或 FIPS 模式？请勿依赖 ed25519"
    这些配置档会将系统的 *加密策略* 切换为 **FIPS**，这**不接受 ed25519 密钥**（也不接受 FIPS 集合之外的密码套件）。请**在加固之前**生成 **RSA**（`ssh-keygen -t rsa -b 3072`）或 **ECDSA**（`ssh-keygen -t ecdsa -b 384`）密钥，并确认新的登录有效。对于默认基线，ed25519 仍然非常安全。参见[一键加固](hardening.md)。

## 生产环境最佳实践

- 最小化暴露面：仅在 `firewalld` 中开放必要的端口。
- 通过 VPN/隧道访问 Cockpit，不要将其直接暴露在互联网上。
- 保持系统更新（`dnf upgrade`）—— 获取 Fedora 安全更新。
- 定期运行 `herd-compliance-scan` 并处理对你的环境有意义的部分。

## 静态数据加密与 FIPS（可选）

有两项**默认关闭**的增强功能（以免对无头服务器和云环境造成阻碍），你可以在策略要求时启用它们。

### 磁盘加密 (LUKS)

保护**静态**数据（被盗/丢弃的磁盘不会泄露任何信息）。这是一个**安装时**的决定。由于 Herd 安装程序是**文本模式**的（没有图形版本中的“加密我的数据”复选框），加密是通过 **kickstart** 设置的 —— 自 **1.0.1** 起，`cryptsetup` 包已包含在 ISO 负载中，因此加密安装支持离线工作。使用 [Herd kickstart](installation-iso.md) 模板：

```text
clearpart --all --initlabel
autopart --type=lvm --encrypted --passphrase=YOUR-PASSPHRASE
```

通过以下方式让安装程序指向该 kickstart：构建你自己的 ISO（`KS=my.ks ./build-iso.sh`，参见[仓库](https://github.com/capivaraos/capivaraos-herd)）或在启动时传入 `inst.ks=<url>`。

安装后验证：

```bash
lsblk                       # 设备应显示为 "crypt"
sudo cryptsetup status luks-<uuid>
```

!!! warning "无头服务器：每次启动都会要求输入密码"
    LUKS 卷在**每次启动时**都会提示输入**密码**。如果没有控制台（物理机/IPMI/串口），机器在重启后将**无法自行启动**。对于服务器自动解锁，请使用 **NBDE** —— `clevis` 配合 **TPM2**（本地密封）或 **Tang**（网络密钥共享）。**不要**在**云**镜像（qcow2）上使用 LUKS。

### FIPS 模式

FIPS 140 强制仅使用经过验证的加密模块，并限制允许的算法。这对政府/行业要求很有用。

!!! warning "FIPS 模式不等于 FIPS 认证"
    开启 `fips=1` 会使系统进入 **FIPS 模式**：它仅使用基础系统为该模式提供的加密模块，并拒绝允许集合之外的算法。这仅仅是**配置**，而非验证：启用该模式并不能使 Herd 或基础系统获得“FIPS 认证”。CMVP 验证是一个正式流程，由认可的实验室针对特定版本的模块进行。如果你的要求需要**经过 CMVP 验证**的模块，请根据 [NIST 的 CMVP](https://csrc.nist.gov/projects/cryptographic-module-validation-program/validated-modules) 检查你运行的确切版本 —— 开启该模式并不证明已通过验证。另请参阅[一键加固](hardening.md)中的合规注意事项。

!!! note "Fedora 44 不再提供 `fips-mode-setup`"
    Fedora [移除了 `fips-mode-setup`](https://fedoraproject.org/wiki/Changes/RemoveFipsModeSetup)。现在通过在**内核命令行**上使用 **`fips=1`** 来开启 FIPS —— **最好在安装时进行**（根据 Fedora 的说法，安装后启用“会存在细微的 bug，不再推荐”）。initramfs 已经包含 `fips` 模块，当内核处于 FIPS 模式时，加密策略会自动切换到 FIPS。

**推荐 —— 在安装时：** 在 ISO 启动菜单中，编辑条目（`e`/`Tab`）并在内核行追加 **`fips=1`**。安装后的系统即处于 FIPS 模式。

**安装后（附上述注意事项）：** 在 Herd 中，`/boot` 是一个单独的分区，因此 `fips=1` 必须与 `boot=UUID=<uuid of /boot>` 一起使用。以下代码块会为你检测 UUID：

```bash
if findmnt /boot >/dev/null 2>&1; then
  UUID=$(sudo blkid -s UUID -o value "$(findmnt -no SOURCE /boot)")
  ARGS="fips=1 boot=UUID=$UUID"
else
  ARGS="fips=1"
fi
sudo grubby --update-kernel=ALL --args="$ARGS"
sudo reboot
```

验证：

```bash
cat /proc/sys/crypto/fips_enabled     # 1 = 已激活
update-crypto-policies --show          # FIPS
```

要禁用：移除 `fips=1`（`sudo grubby --update-kernel=ALL --remove-args="fips=1"`）并重启。

!!! warning "依赖前请先测试访问"
    FIPS 模式会**限制算法**：特别是，**将不再接受 ed25519 SSH 密钥**（请使用 **RSA** 或 **ECDSA**），以及允许集合之外的密码套件和证书。**请在重启后立即在第二个会话中确认 SSH 登录成功**，然后再关闭当前的访问会话。

## 第三方声明与许可

`herd-compliance-scan` 使用了 **[SCAP 安全指南](https://github.com/ComplianceAsCode/content)**（ComplianceAsCode 项目）。Herd 重新分发了用于 Fedora 的 SSG 数据流 (`ssg-fedora-ds.xml`)，其采用 **BSD-3-Clause** 许可证：

> Copyright (c) 2012-2017, Red Hat, Inc. All rights reserved.
>
> 在满足 BSD-3-Clause 条件（保留版权声明、条件和免责声明；不得使用持有人的姓名进行背书）的前提下，允许以源代码和二进制形式重新分发和使用，无论是否经过修改。完整文本随 `capivaraos-herd-hardening` 包一起提供，位于 `/usr/share/licenses/capivaraos-herd-hardening/`，也可在 [SSG 仓库](https://github.com/ComplianceAsCode/content/blob/master/LICENSE)中获取。

配置档名称（`standard`、`cis`、`cis_server_l1` 等）来自 SSG 本身。“CIS”指的是[互联网安全中心](https://www.cisecurity.org/)的基准；Herd 不复制基准文本，它仅运行 SSG 提供的相应配置档。

---

*内容在 AI 协助下生成，并处于持续审核中。发现错误？[提交 issue](https://github.com/capivaraos/capivaraos-herd/issues)。*