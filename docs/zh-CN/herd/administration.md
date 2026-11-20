# 系统管理

通过终端对 Herd 服务器进行日常管理任务。几乎所有操作也可以在 [Cockpit](cockpit.md) 中以图形化方式完成。

## 更新

Herd 使用 **dnf**（基于 Fedora）：

```bash
sudo dnf upgrade --refresh          # 升级所有包
sudo dnf upgrade --security         # 仅安装安全更新
sudo dnf check-update               # 查看有哪些更新
```

内核更新后重启：`sudo systemctl reboot`。

## 服务管理 (systemd)

```bash
systemctl status sshd               # 查看服务状态
sudo systemctl restart sshd         # 重启服务
sudo systemctl enable --now nginx   # 设置开机自启并立即启动
systemctl list-units --type=service --state=running
```

查看服务日志：`journalctl -u sshd`。

## 日志 (journal)

```bash
journalctl -b                       # 本次启动的日志
journalctl -p err -b                # 仅查看错误信息
journalctl -u <service> -f          # 实时跟踪服务日志
journalctl --since "today"          # 查看今天的日志
```

## 存储

```bash
lsblk                               # 查看磁盘和分区
df -h                               # 查看文件系统使用情况
sudo lvs; sudo vgs; sudo pvs        # LVM（默认安装使用 LVM）
```

要添加/扩展磁盘、LVM 和挂载点，使用 [Cockpit](cockpit.md) 中的**存储**模块是最简单的方法。

## 网络

由 NetworkManager (`nmcli`) 管理 —— 参见[快速入门 › 网络](getting-started.md#network)中的示例。

```bash
nmcli device status
nmcli connection show
ip a                                # 查看当前 IP 地址
```

## <a id="firewall"></a>防火墙 (firewalld)

默认情况下，`public` 区域允许 **ssh**、**cockpit** 和 **dhcpv6-client**。

```bash
sudo firewall-cmd --list-all                                   # 查看区域状态
sudo firewall-cmd --permanent --add-service=https              # 允许服务
sudo firewall-cmd --permanent --add-port=8080/tcp             # 允许端口
sudo firewall-cmd --reload                                     # 应用配置
```

仅允许来自管理网络的端口访问（比向所有人开放更安全）：

```bash
sudo firewall-cmd --permanent --zone=internal --add-source=192.168.10.0/24
sudo firewall-cmd --permanent --zone=internal --add-service=cockpit
sudo firewall-cmd --reload
```

## SELinux

默认处于 `enforcing` 模式（建议保持此状态）：

```bash
getenforce
sudo ausearch -m avc -ts recent     # 最近的拒绝记录
sudo sealert -a /var/log/audit/audit.log   # 分析日志（如果安装了 setroubleshoot）
```

!!! warning "不要禁用 SELinux"
    如果某个服务被拦截，请优先修复正确的上下文/布尔值，而不是禁用 SELinux。禁用它将会移除一个重要的安全隔离层。

## 关机 / 重启

```bash
sudo systemctl reboot
sudo systemctl poweroff
```

## 下一步

- [安全与合规](security.md)
- [技术支持](support.md)

---

*内容在 AI 协助下生成，并处于持续审核中。发现错误？[提交 issue](https://github.com/capivaraos/capivaraos-herd/issues)。*