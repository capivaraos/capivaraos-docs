# 快速入门

安装完成后，以下是针对 Herd 服务器的一些初始调整。这里的所有操作都可以通过终端（SSH/控制台）或 [Cockpit](cockpit.md) 完成。

## 访问

- **SSH（通过密钥）：** `ssh user@<ip>` —— 请记住 SSH 是**仅限密钥认证**的（参见[通过 ISO 安装 › 远程访问](installation-iso.md#remote-access)）。
- **控制台：** 物理机/IPMI，使用管理员用户或 root。
- **Cockpit：** `https://<ip>:9090`（用户名 + 密码）。

## 提权 (sudo)

管理员用户（属于 `wheel` 组）使用 `sudo`：

```bash
sudo dnf upgrade --refresh
```

## 更新系统

首要任务 —— 将系统更新到最新状态：

```bash
sudo dnf upgrade --refresh
sudo systemctl reboot   # 如果安装了新内核
```

## 主机名

```bash
sudo hostnamectl set-hostname herd-01.example.com
```

## 网络

网络通过 **DHCP** 获取。如需设置静态 IP，请使用 NetworkManager (`nmcli`)：

```bash
# 列出连接和接口
nmcli connection show
nmcli device status

# 设置静态 IP（请根据实际情况修改连接名称和数值）
sudo nmcli connection modify "System eth0" \
  ipv4.method manual \
  ipv4.addresses 192.168.0.10/24 \
  ipv4.gateway 192.168.0.1 \
  ipv4.dns "1.1.1.1 8.8.8.8"

sudo nmcli connection up "System eth0"
```

!!! tip "通过 Cockpit 操作"
    你同样可以在 [Cockpit](cockpit.md) 的**网络 (Networking)** 模块中，通过图形界面完成这些操作。

## 时区与时间

```bash
timedatectl                              # 查看当前状态
sudo timedatectl set-timezone America/Sao_Paulo
```

时间同步 (`chronyd`) 已默认启用。

## 语言环境

系统自带 `pt_BR.UTF-8` 和 `en_US.UTF-8`。要更改默认语言：

```bash
localectl                                # 查看当前状态
sudo localectl set-locale LANG=en_US.UTF-8
```

## 用户与 SSH 密钥

创建另一个管理员用户：

```bash
sudo useradd -m -G wheel mary
sudo passwd mary
# 添加其公钥：
sudo install -d -m 700 /home/mary/.ssh
echo "ssh-ed25519 AAAA... mary@pc" | sudo tee -a /home/mary/.ssh/authorized_keys
sudo chown -R mary:mary /home/mary/.ssh
sudo chmod 600 /home/mary/.ssh/authorized_keys
```

你也可以在 [Cockpit](cockpit.md) 的**账户 (Accounts)** 模块中管理用户。

## 为应用程序开放端口

默认情况下，`firewalld` 仅允许 **SSH** 和 **Cockpit**。例如，若要提供 Web 服务：

```bash
sudo firewall-cmd --permanent --add-service=http --add-service=https
sudo firewall-cmd --reload
sudo firewall-cmd --list-all
```

更多内容请参阅[系统管理 › 防火墙](administration.md#firewall)。

## 下一步

- [Web 控制台 (Cockpit)](cockpit.md)
- [安全与合规](security.md) —— 运行 `herd-compliance-scan`。
- [系统管理](administration.md) —— 服务、存储、日志、更新。

---

*内容在 AI 协助下生成，并处于持续审核中。发现错误？[提交 issue](https://github.com/capivaraos/capivaraos-herd/issues)。*