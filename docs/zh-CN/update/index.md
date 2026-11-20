# 保持系统更新

已安装的 CapivaraOS 可以保持更新，**无需重新格式化**，也无需等待新的 ISO。更新分为两种类型，使用不同的工具：

| 类型 | 变更内容 | 工具 |
|---|---|---|
| **日常更新** | 同一代系统内的修复和新程序版本 | 应用商店或 `dnf upgrade` |
| **跨代升级** | 升级到系统的下一代（新的 Fedora 基础系统） | `capivaraos-upgrade` |

!!! note "本页面针对的是桌面版"
    Marsh、Pup 和 Snout 使用 `capivaraos-upgrade`，并且确实会自动创建 Btrfs 快照。**Herd** 使用 `herd-upgrade`，并且在常见的服务器布局（LVM + xfs）下**不会**创建自动快照 —— 参见[保持 Herd 更新](../herd/update.md)。

在这两种更新中，除了 Fedora 的软件包之外，你还会从**已签名的软件仓库**中接收**我们的**软件包（品牌标识、COS Dev Center、主题、修复补丁）。信任不依赖于对服务器的信任：每个软件包**以及**元数据都会使用我们的 GPG 密钥进行验证（`gpgcheck` + `repo_gpgcheck`）。该密钥的私钥部分保存在保险库中，仅在签署版本的那一刻进入机器。这与[验证你的下载](../installation/verify-download.md)使用的是同一把密钥。

## 日常更新

这些更新会通过各个版本的应用商店自动送达 —— 出现时直接应用即可：

| 版本 | 位置 |
|---|---|
| **Marsh** (KDE) | **Discover** (Discover软件) |
| **Snout** (GNOME) | **GNOME Software** (GNOME 软件) |
| **Pup** (Xfce) | 通过终端 —— 参见下方的警告 |

在任何版本上，通过终端执行以下命令始终有效：

```bash
sudo dnf upgrade --refresh
```

!!! warning "Pup：请从终端进行更新"
    在 Fedora 45 基础系统上，Xfce 的图形化更新程序可能**不会列出**我们的更新（这是该组件的限制，而非我们仓库的问题）。在 Pup 上，请优先使用 `sudo dnf upgrade --refresh` —— 在上游修复图形化更新程序之前，这是最可靠的方法。

## 跨代升级（原地升级）

当 CapivaraOS 发布**新的一代**时，你可以使用 `capivaraos-upgrade` 进行原地升级，无需重装。它使用 `dnf5` 的**离线**事务（`system-upgrade`）：在系统运行时下载所有内容，并在下次启动时在隔离的环境中应用它们。

它的设计旨在确保行为可预测：默认情况下它只进行**检查**，下载和重启是**分开的、明确的**步骤。

```bash
# 1. 预检：显示将会发生什么。不下载也不重启。
capivaraos-upgrade

# 2. 下载事务（系统保持运行；不重启）：
sudo capivaraos-upgrade download

# 3. 应用：重启并在离线环境中执行升级。
sudo capivaraos-upgrade reboot
```

!!! info "故障关闭（Fail-closed）：不会让你停留在半升级状态"
    只有当目标代**已存在我们签名的软件仓库**时，升级才会继续。如果尚未发布，它会**拒绝**执行 —— 否则，机器将迁移到新的 Fedora 基础系统，而把 CapivaraOS 的软件包（品牌标识、COS Dev、主题）落下。因此，**现在**运行 `capivaraos-upgrade` 通常会提示尚未发布目标代：这是正确的行为，而非错误。

### 升级前：创建快照

当磁盘支持时，该工具会**首先创建一个快照**，以便你可以回滚：

- **btrfs + snapper**：**自动**创建快照。这是安装程序在三个版本上的默认布局。
- **LVM**：非自动 —— 该工具会在预检中打印出需要手动执行的命令（`lvcreate -s …`）。
- **没有兼容的存储**：不进行任何快照 —— 请**先备份**。

!!! tip "预检会告诉你机器上将会发生什么"
    无需猜测你处于哪种情况：直接运行不带参数的 `capivaraos-upgrade`。它会在下载任何内容之前，报告是否会创建快照以及在什么条件下创建。

### 如果需要回退

该工具创建的快照会显示在列表中，描述为 **"pré capivaraos-upgrade"** —— 这就是你的返回点：

```bash
sudo snapper list
```

要撤销从该时间点至今的文件更改：

```bash
sudo snapper undochange <snapshot#>..0
```

`0` 表示“当前状态”。要在跨代升级后**恢复整个系统**，路径是**启动该快照并将其提升为主系统** —— `undochange` 仅回退文件更改，它不会切换整个系统。

在 **LVM** 上，回滚操作是 `sudo lvconvert --merge <snapshot>` 然后重启 —— 但前提是你事先手动创建了 LVM 快照。LVM 快照不是自动的，需要卷组中有可用空间。

## 为什么 Series 1 不能升级到 Series 2

Series 1 已达到**生命周期结束 (EOL)**，既没有已签名的软件仓库，也没有原地升级引擎（两者都是在 Series 2 中诞生的）。因此，无法从 Series 1 原地跳转到 Series 2：如果你使用的是 Series 1，请**全新安装** Series 2 —— 参见[验证你的下载](../installation/verify-download.md)和[安装指南](../installation/guide.md)。请先备份你的数据。Series 1 的文档仍可在[归档](../arquivo/1.x/index.md)中查看。