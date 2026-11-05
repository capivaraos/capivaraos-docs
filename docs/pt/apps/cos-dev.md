# COS Dev Center

O **COS Dev Center** abre sozinho no primeiro boot e pergunta o que você precisa. Em vez de caçar tutorial para instalar IA local, Node.js ou Docker, você escolhe numa lista e ele faz — pedindo a senha de administrador só quando é realmente necessário.

Ele é exclusivo do CapivaraOS e vem instalado nas três edições desktop. Para abrir depois, procure por **COS Dev Center** no menu de aplicativos.

!!! tip "Instale no disco antes de usar"
    Vários itens baixam alguns gigabytes. Numa sessão *live* (rodando pelo pendrive) o espaço é limitado e tudo se perde ao desligar. Instale o sistema primeiro.

## O que ele oferece

### IA local

Instala a pilha para rodar modelos de IA **na sua máquina, sem nuvem e sem conta**:

- **Ollama** — o motor que executa os modelos
- **Alpaca** — interface gráfica para conversar com eles
- **Continue** — extensão que leva a IA para dentro do editor

!!! warning "Isto baixa vários gigabytes"
    No Fedora 45 o pacote `ollama` depende do `ollama-rocm`, o suporte a GPU AMD. Ele é instalado **mesmo em máquinas sem placa AMD**, e sozinho são alguns gigabytes. Não é escolha nossa: é como o pacote está montado no Fedora. Reserve espaço e uma conexão à vontade.

Sem GPU dedicada os modelos rodam na CPU. Funciona, mas é mais lento — prefira modelos menores.

### Trilhas de linguagem

Instala linguagens **no seu usuário**, via `mise`, sem alterar o sistema:

Web/JavaScript (Node.js) · Python · Go · Rust · Java

Como nada é instalado no sistema, você pode ter versões diferentes por projeto e remover tudo sem deixar rastro. **Abra um terminal novo depois de instalar**, senão o `PATH` ainda é o antigo.

### Ligar GPU (NVIDIA)

Instala o driver proprietário da NVIDIA e o CUDA pelo RPM Fusion, para acelerar a IA local.

É **só para NVIDIA**. Em placas AMD a aceleração já vem junto da IA local, pelo ROCm. Sem GPU dedicada, não há o que ligar — a IA roda na CPU.

### Ferramentas de dev

Você escolhe quais instalar:

- **VS Code** — o editor da Microsoft (instalado no sistema, pede senha)
- **Cline** — agente de IA dentro do editor
- **Open WebUI** — interface web de conversa para o Ollama, em contêiner podman
- **Docker CE** — contêineres (instalado no sistema, pede senha)

### GitHub CLI + pre-commit

Configura o `gh`, para usar o GitHub pelo terminal, e instala um modelo de *pre-commit* com verificações de segurança antes de cada commit: **gitleaks** (procura segredo vazando), detecção de chaves e **ShellCheck** nos scripts.

## Como ele se comporta

**Uma tarefa por vez.** Enquanto uma instalação roda, as outras ficam bloqueadas — evita duas transações de pacote disputando o sistema.

**A seção "Instalados"** mostra o que você já instalou por ele, para não repetir.

**A senha é pedida só quando precisa.** Trilhas de linguagem e Open WebUI ficam no seu usuário e não pedem nada. VS Code, Docker e o driver NVIDIA mexem no sistema e pedem.

## Também existe o COS Config Center

O Dev Center cuida do ambiente de desenvolvimento. Para ajustes do sistema — codecs, teclado, idioma, firewall, discos, rede — o aplicativo é o [COS Config Center](cos-config.md).
