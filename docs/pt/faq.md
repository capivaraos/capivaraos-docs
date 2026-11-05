# Perguntas frequentes

## O CapivaraOS é gratuito?

Sim, completamente. É um projeto open source, sem custo para baixar, instalar ou usar.

## Qual a diferença entre Marsh, Pup e Snout?

A diferença está no ambiente gráfico:

- **Marsh** usa KDE Plasma — mais completo e personalizável
- **Pup** usa Xfce — mais leve, ótimo para hardware modesto
- **Snout** usa GNOME — limpo e sem modificações de layout

O sistema base, a identidade visual, os dois aplicativos exclusivos ([COS Dev Center](apps/cos-dev.md) e [COS Config Center](apps/cos-config.md)) e o conjunto de ferramentas de desenvolvimento são os mesmos nas três.

## O CapivaraOS substitui o Fedora?

Não exatamente. O CapivaraOS é construído sobre o Fedora — ele herda o sistema de pacotes, o ciclo de atualizações e a compatibilidade de hardware do Fedora, mas adiciona identidade visual própria, dois aplicativos exclusivos e um ambiente de desenvolvimento pronto no primeiro boot.

## Consigo instalar programas do Fedora no CapivaraOS?

Sim. Como a base é Fedora, todos os repositórios e pacotes do Fedora funcionam normalmente. O DNF e o Flatpak estão disponíveis.

## O CapivaraOS tem suporte a Flatpak?

Sim, o Flatpak já vem configurado. Você pode instalar aplicativos do Flathub normalmente.

## Com que frequência saem atualizações?

O CapivaraOS segue o ciclo do Fedora. Atualizações de segurança e de pacotes chegam continuamente, pela loja de aplicativos ou pelo `dnf`.

## Preciso reinstalar para pegar a próxima versão?

Não, a partir da Série 2. O sistema sobe de versão e de geração **no lugar**, sem formatar, e tira um instantâneo antes para você poder voltar atrás. Veja [Atualizar o sistema](atualizar/index.md).

Quem está na Série 1 é a exceção: ela não tem esse caminho e precisa de uma instalação limpa — é a última vez.

## Como sei que a ISO que baixei é mesmo de vocês?

Cada ISO vem com a soma SHA-256 e com uma assinatura dessa soma, feita com a nossa chave. O checksum prova que o arquivo chegou inteiro; a assinatura prova quem publicou. O passo a passo está em [Verificar o download](instalacao/verificar-download.md).

## A IA local manda meus dados para algum lugar?

Não. Os modelos rodam na sua máquina, sem nuvem e sem conta. O que sai da sua rede é só o download inicial dos modelos. Detalhes em [COS Dev Center](apps/cos-dev.md).

## Onde reporto um problema?

No fórum ou abrindo uma issue no GitHub. Veja a página [Como contribuir](contribua.md).

## O CapivaraOS funciona em máquina virtual?

Sim, funciona em VirtualBox, VMware e QEMU/KVM. Para o Marsh e o Snout, recomendamos habilitar a aceleração 3D.

Numa máquina virtual, a IA local roda na CPU — funciona, mas é lenta.

## Posso usar o CapivaraOS em dual boot?

Sim. O instalador Anaconda suporta particionamento manual para dual boot com Windows ou outras distribuições Linux.
