# CapivaraOS HERD

O **CapivaraOS HERD** é a linha de **servidor** do CapivaraOS: um sistema Linux
_headless_ (sem ambiente gráfico), baseado em **Fedora 44**, pensado para rodar
em servidores físicos, máquinas virtuais e na nuvem — com identidade brasileira,
segurança sã por padrão e o **Cockpit** como console de administração no
navegador.

!!! info "Edição Community 1.0.0"
    Esta documentação cobre o **HERD Community**, a edição **gratuita e de código
    aberto**. Arquitetura **x86_64** (o suporte a **aarch64** chega na 1.1).

## A ideia: manada = frota

No CapivaraOS, a capivara vive em **manada** — e é assim que pensamos servidores:
uma **frota** que trabalha junta. O HERD entrega uma base de servidor previsível,
endurecida e fácil de administrar, seja um nó só ou muitos.

## O que vem no HERD Community

| Área | O que você tem |
|---|---|
| **Base** | Fedora 44 (mesma geração das spins de desktop Marsh/Pup/Snout 1.x) |
| **Perfil** | Servidor _headless_ — sem desktop, superfície mínima |
| **Console web** | **Cockpit** em `https://<ip>:9090` (serviços, rede, storage, contas, logs, terminal) |
| **Segurança** | SELinux _enforcing_, `firewalld` restritivo, SSH endurecido, política de senha, `umask 027` |
| **Compliance** | `herd-compliance-scan` — relatório OpenSCAP embarcado (perfil _standard_ do SSG) |
| **Nuvem** | `cloud-init` na imagem qcow2 (injeção de chave/rede/usuário) |
| **Idiomas** | pt_BR e en_US; teclado ABNT2; fuso America/Sao_Paulo (ajustável) |

## Duas formas de instalar

<div class="grid cards" markdown>

-   :material-disc: **ISO instaladora (bare-metal)**

    Para instalar em servidor físico ou VM a partir do zero, com o instalador
    Anaconda em modo texto.

    [:octicons-arrow-right-24: Instalação via ISO](instalacao-iso.md)

-   :material-cloud: **Imagem qcow2 (nuvem/VM)**

    Para subir rápido em nuvem ou virtualização, com `cloud-init` cuidando de
    usuário, chave SSH e rede.

    [:octicons-arrow-right-24: Implantação em nuvem/VM](instalacao-nuvem.md)

</div>

## Por onde começar

1. [Requisitos](requisitos.md) — o que o servidor precisa ter.
2. [Download e verificação](download.md) — baixar a imagem e conferir a integridade.
3. [Instalação via ISO](instalacao-iso.md) **ou** [nuvem/VM](instalacao-nuvem.md).
4. [Primeiros passos](primeiros-passos.md) — acesso, rede, atualizações.
5. [Console web (Cockpit)](cockpit.md) — administrar pelo navegador.
6. [Segurança e compliance](seguranca.md) — o que vem endurecido e como auditar.

!!! tip "Já instalou?"
    Vá direto para os [Primeiros passos](primeiros-passos.md) ou para o
    [Console web (Cockpit)](cockpit.md).

## Suporte e comunidade

Dúvidas, bugs e sugestões: veja a página de [Suporte](suporte.md). O HERD
Community é mantido pela comunidade — sua participação é bem-vinda.

---

*Conteúdo produzido com auxílio de IA, em revisão contínua. Encontrou um erro? [Abra uma issue](https://github.com/capivaraos/capivaraos-herd/issues).*
