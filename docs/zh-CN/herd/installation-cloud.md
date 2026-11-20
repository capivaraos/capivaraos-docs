# 云 / 虚拟机部署 (qcow2)

**qcow2** 镜像已自带 `cloud-init` 和 `qemu-guest-agent`，这使其非常适合在云环境、KVM/libvirt、Proxmox 等平台上快速启动。`cloud-init` 会在首次启动时处理**用户**、**SSH 密钥**、**主机名**和**网络**配置。

!!! info "镜像 = 模板"
    qcow2 镜像是一个 _模板_。从中创建的每个虚拟机都会在首次启动时由 `cloud-init`（从数据源）进行个性化配置。

## 选项 A —— 使用 NoCloud seed 配合 KVM/libvirt

在没有云服务商的情况下，向 `cloud-init` 提供配置的最简单方式是使用 **NoCloud** 数据源：一个包含 `meta-data` 和 `user-data` 两个文件的小型 ISO 镜像。

### 1. 创建 seed 文件

`meta-data`：

```yaml
instance-id: herd-01
local-hostname: herd-01
```

`user-data`（注意第一行的 `#cloud-config`）：

```yaml
#cloud-config
users:
  - name: admin
    groups: [wheel]
    sudo: "ALL=(ALL) NOPASSWD:ALL"
    shell: /bin/bash
    ssh_authorized_keys:
      - ssh-ed25519 AAAA... you@machine
# 可选密码 — 用于控制台和 Cockpit（SSH 仅限密钥）
chpasswd:
  expire: false
  users:
    - name: admin
      password: "change-this-password"
      type: text
```

!!! warning "SSH 仅限密钥认证"
    Herd 默认配置为 `PasswordAuthentication no`。请**在 `ssh_authorized_keys` 中包含你的公钥**，否则你将无法通过 SSH 登录。密码（如果设置了）是用于控制台和 [Cockpit](cockpit.md) 登录的。

!!! note "免密码 sudo"
    此示例使用了 `NOPASSWD:ALL`（无需输入密码即可使用 sudo）—— 这在仅限密钥登录的云环境中很方便。如果需要更严格的控制，请改为 `ALL=(ALL) ALL` 并设置用户密码；这样 `sudo` 就会要求输入密码。

### 2. 构建 seed ISO

```bash
genisoimage -output seed.iso -volid cidata -joliet -rock user-data meta-data
# （或者：cloud-localds seed.iso user-data meta-data）
```

### 3. 启动虚拟机

```bash
cp CapivaraOS-HERD-1.0.1-x86_64.qcow2 herd-01.qcow2

virt-install --name herd-01 --memory 2048 --vcpus 2 \
  --disk path=herd-01.qcow2,format=qcow2,bus=virtio \
  --disk path=seed.iso,device=cdrom \
  --os-variant fedora-unknown --import \
  --network network=default,model=virtio --graphics none
```

查找 IP（`virsh domifaddr herd-01`）并连接：`ssh admin@<ip>`。

## 选项 B —— 云服务商

1. **上传 qcow2** 作为你的服务商平台上的镜像/模板（例如 OpenStack Glance 等）—— 或者将其作为磁盘导入（Proxmox：`qm importdisk`）。
2. 从该镜像创建实例。
3. 通过服务商的机制（元数据服务 / ConfigDrive）提供 **user-data**（与上面相同的 `#cloud-config`）。`cloud-init` 会在首次启动时应用它。

!!! tip "qemu-guest-agent"
    已安装并启用 —— 虚拟化宿主机可以查询 IP、冻结快照并优雅关机。

## 启动之后

- [快速入门](getting-started.md) —— 更新、网络、时区。
- [Web 控制台 (Cockpit)](cockpit.md) —— `https://<ip>:9090`。
- [安全与合规](security.md)。

---

*内容在 AI 协助下生成，并处于持续审核中。发现错误？[提交 issue](https://github.com/capivaraos/capivaraos-herd/issues)。*