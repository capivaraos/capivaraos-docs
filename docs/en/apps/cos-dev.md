# COS Dev Center

**COS Dev Center** opens by itself at first boot and asks what you need. Instead of hunting for a tutorial to install local AI, Node.js or Docker, you pick from a list and it does the work — asking for the administrator password only when it genuinely has to.

It is exclusive to CapivaraOS and ships on all three desktop editions. To open it later, look for **COS Dev Center** in the application menu.

!!! tip "Install to disk before using it"
    Several items download multiple gigabytes. In a *live* session (running from the USB stick) space is limited and everything is lost at shutdown. Install the system first.

## What it offers

### Local AI

Installs the stack to run AI models **on your own machine, with no cloud and no account**:

- **Ollama** — the engine that runs the models
- **Alpaca** — a graphical interface to talk to them
- **Continue** — the extension that brings AI into the editor

!!! warning "This downloads several gigabytes"
    On Fedora 45 the `ollama` package depends on `ollama-rocm`, the AMD GPU support. It gets installed **even on machines with no AMD card**, and it alone is a few gigabytes. This is not our choice: it is how the package is built in Fedora. Make room and give it a decent connection.

Without a dedicated GPU the models run on the CPU. That works, but it is slower — prefer smaller models.

### Language tracks

Installs languages **in your user account**, through `mise`, without touching the system:

Web/JavaScript (Node.js) · Python · Go · Rust · Java

Because nothing goes into the system, you can keep different versions per project and remove everything without a trace. **Open a new terminal after installing**, or your `PATH` is still the old one.

### Enable GPU (NVIDIA)

Installs the proprietary NVIDIA driver and CUDA from RPM Fusion, to accelerate local AI.

It is **NVIDIA only**. On AMD cards the acceleration already comes with local AI, through ROCm. With no dedicated GPU there is nothing to enable — AI runs on the CPU.

### Dev tools

You choose which ones to install:

- **VS Code** — Microsoft's editor (system-wide, asks for the password)
- **Cline** — an AI agent inside the editor
- **Open WebUI** — a web chat interface for Ollama, in a podman container
- **Docker CE** — containers (system-wide, asks for the password)

### GitHub CLI + pre-commit

Sets up `gh`, for using GitHub from the terminal, and installs a *pre-commit* template with security checks that run before every commit: **gitleaks** (looks for leaking secrets), key detection and **ShellCheck** on scripts.

## How it behaves

**One task at a time.** While an installation runs, the others are blocked — this keeps two package transactions from fighting over the system.

**The "Installed" section** shows what you already installed through it, so you do not repeat yourself.

**The password is asked only when needed.** Language tracks and Open WebUI live in your user account and ask for nothing. VS Code, Docker and the NVIDIA driver touch the system and do ask.

## There is also COS Config Center

The Dev Center takes care of the development environment. For system settings — codecs, keyboard, language, firewall, disks, network — the application is [COS Config Center](cos-config.md).
