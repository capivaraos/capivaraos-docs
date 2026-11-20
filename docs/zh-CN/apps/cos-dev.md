# COS Dev Center

**COS Dev Center** 首次开机时会自动弹出来，问你需要什么。不用到处翻教程装本地 AI、Node.js 或 Docker，从清单里挑一下，它就替你办了 — 只有真有必要的时候才会要管理员密码。

它是 CapivaraOS 独有的，三个桌面版本都预装了。想再打开，在应用菜单里找 **COS Dev Center** 就行。

!!! tip "先装到硬盘再用"
    好几个项目要下载好几 GB。在 *live* 会话里（从 U 盘启动）空间有限，关机后一切都会丢。先把系统装到硬盘上。

## 它能装什么

### 本地 AI

装上这一整套，让 AI 模型 **在你自己的机器上跑，不靠云、也不用账号**：

- **Ollama** — 跑模型的引擎
- **Alpaca** — 跟模型聊天的图形界面
- **Continue** — 把 AI 带进编辑器的扩展

!!! warning "这会下载好几 GB"
    在 Fedora 45 上，`ollama` 包依赖 `ollama-rocm`（也就是 AMD GPU 支持）。**就算机器上没有 AMD 显卡也会装上**，而且单它一个就占了几 GB 的空间。这不是我们的决定，这是 Fedora 中该软件包的构建方式。请腾出足够的存储空间，并确保有良好的网络连接。

没有独立显卡的话，模型就跑在 CPU 上。能用，但会慢一些 — 挑小一点的模型更合适。

### 语言工具链

把语言装 **在你的用户账户里**，通过 `mise`，不动系统：

Web/JavaScript (Node.js) · Python · Go · Rust · Java

因为什么都没装进系统，你可以给每个项目留不同版本，也能删得干干净净不留痕迹。**装完记得开个新终端**，不然 `PATH` 还是旧的。

### 开启 GPU（NVIDIA）

从 RPM Fusion 装上 NVIDIA 私有驱动和 CUDA，给本地 AI 加速。

这个选项**仅针对 NVIDIA 显卡**。在 AMD 显卡上，本地 AI 已经通过 ROCm 提供了加速支持。如果机器没有独立显卡，则无需（额外）启用任何选项——AI 会直接运行在 CPU 上。

### 开发工具

请选择要安装的组件：

- **VS Code** — 微软的编辑器（全局安装，需输入管理员密码）
- **Cline** — 编辑器里的 AI 智能体
- **Open WebUI** — 给 Ollama 用的网页聊天界面，跑在 podman 容器里
- **Docker CE** — 容器（全局安装，需输入管理员密码）

### GitHub CLI + pre-commit

配好 `gh`，让你在终端里用 GitHub；再装一个 *pre-commit* 模板，每次提交前都会跑一遍安全检查：**gitleaks**（查有没有密钥泄露）、密钥检测，以及脚本上的 **ShellCheck**。

## 它怎么工作

**一次只做一件事。** 装一个的时候，其他都先排着 — 防止两个包事务抢着改系统。

**"已安装"那一栏**列出你已经通过它装过的东西，这样就不会重复安装了。

**只在需要时才要密码。** 语言工具链和 Open WebUI 待在你的用户账户里，什么都不问。VS Code、Docker 和 NVIDIA 驱动要动系统才会询问密码。

## 还有 COS Config Center

Dev Center 负责管理开发环境。系统设置 — 编解码器、键盘、语言、防火墙、磁盘、网络 — 则由 [COS Config Center](cos-config.md)应用负责管理。
