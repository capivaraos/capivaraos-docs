# Pup

**Ambiente:** Xfce 4.20 · **Base:** Fedora 45 · **Arquitetura:** x86_64

O Pup é a edição leve. O Xfce consome pouca memória e roda bem em máquina antiga, mas aqui ele não é uma versão reduzida: o Pup traz o **mesmo conjunto de ferramentas de desenvolvimento** das outras duas edições.

É a escolha para hardware modesto, e também para quem simplesmente prefere um desktop mais direto.

## O que vem instalado

- **Xfce 4.20**
- **Firefox** como navegador padrão
- **LibreOffice**, com tradução para português
- **VLC** para vídeo e áudio
- **Snapper**, para os instantâneos em Btrfs
- **COS Dev Center** e **COS Config Center**, exclusivos do CapivaraOS

O Pup é deliberadamente enxuto: aplicativos pesados ficam de fora por padrão e você instala o que quiser.

## Para desenvolvedores

O [COS Dev Center](../apps/cos-dev.md) abre no primeiro boot com a mesma oferta das outras edições — IA local, trilhas de linguagem, ferramentas de dev e GitHub.

!!! note "IA local em máquina modesta"
    A IA local baixa vários gigabytes e, sem GPU dedicada, roda na CPU. Num computador antigo isso funciona, mas é lento: prefira modelos menores, ou deixe esse item de lado.

## Identidade visual

Mesma identidade das outras edições — Plymouth, GRUB, ícones e tema coerentes —, com coleção própria de wallpapers, incluindo uma exclusiva do Pup.

## Atualização

A partir da Série 2 o Pup atualiza **sem reinstalar**. Veja [Atualizar o sistema](../atualizar/index.md).

!!! warning "No Pup, atualize pelo terminal"
    O `dnfdragora`, atualizador padrão do Xfce, **não lista as atualizações do CapivaraOS** no Fedora 45. Não é problema do nosso repositório — é o `dnfdaemon` que falha ao verificar a assinatura. Até isso ser resolvido, use `sudo dnf upgrade` no terminal. Os detalhes estão em [Atualizar o sistema](../atualizar/index.md).

## Requisitos

Veja [Requisitos de hardware](../instalacao/requisitos.md). O Pup é o que pede menos das três edições.

## Download

[Baixar Pup](https://capivaraos.org/pt/download.html){ .md-button .md-button--primary }

Confira a ISO antes de gravar: [Verificar o download](../instalacao/verificar-download.md).
