# Atualizar o sistema

O CapivaraOS instalado se mantém atualizado **sem formatar** e sem esperar uma
ISO nova. São dois tipos de atualização, com ferramentas diferentes:

| Tipo | O que muda | Ferramenta |
|---|---|---|
| **Atualizações do dia a dia** | correções e novas versões de programas, dentro da mesma geração | a loja de aplicativos ou `dnf upgrade` |
| **Upgrade de geração** | salto para a próxima geração do sistema (base Fedora nova) | `capivaraos-upgrade` |

Em ambos, além dos pacotes do Fedora, você recebe os **nossos** pacotes
(identidade visual, COS Dev Center, temas, correções) a partir de um
**repositório assinado**. A confiança não depende de confiar no servidor: cada
pacote **e** os metadados são verificados contra a nossa chave GPG
(`gpgcheck` + `repo_gpgcheck`). A parte privada da chave é guardada em cofre e só
entra numa máquina no momento de assinar uma publicação. É a mesma chave da
[verificação do download](../instalacao/verificar-download.md).

## Atualizações do dia a dia

Vêm sozinhas pela loja de aplicativos de cada edição — basta aplicar quando
aparecerem:

| Edição | Onde |
|---|---|
| **Marsh** (KDE) | **Discover** |
| **Snout** (GNOME) | **Software do GNOME** |
| **Pup** (Xfce) | pelo terminal — veja o aviso abaixo |

Pelo terminal, em qualquer edição, funciona sempre:

```bash
sudo dnf upgrade --refresh
```

!!! warning "Pup: atualize pelo terminal"
    Na base do Fedora 45, o atualizador gráfico do Xfce pode **não listar** as
    nossas atualizações (uma limitação do próprio componente, não do nosso
    repositório). No Pup, prefira `sudo dnf upgrade --refresh` — é o caminho
    confiável enquanto o atualizador gráfico não é corrigido upstream.

## Upgrade de geração (in-place)

Quando uma **nova geração** do CapivaraOS é publicada, você salta para ela sem
reinstalar, com a ferramenta `capivaraos-upgrade`. Ela usa a transação **offline**
do `dnf5` (`system-upgrade`): baixa tudo com o sistema no ar e aplica no próximo
boot, num ambiente isolado.

Ela foi feita para não te dar susto: por padrão ela só **verifica**, e baixar e
reiniciar são passos **separados e explícitos**.

```bash
# 1. Pré-voo: mostra o que aconteceria. Não baixa nem reinicia nada.
capivaraos-upgrade

# 2. Baixa a transação (o sistema segue no ar; não reinicia):
sudo capivaraos-upgrade download

# 3. Aplica: reinicia e faz o upgrade no ambiente offline.
sudo capivaraos-upgrade reboot
```

!!! info "Falha fechada: não deixa você pela metade"
    O upgrade só prossegue se o **nosso repositório assinado já existir** para a
    geração de destino. Se ainda não existir, ele **recusa** — do contrário a
    máquina subiria a base Fedora nova e deixaria os pacotes do CapivaraOS
    (identidade, COS Dev, temas) para trás. Por isso, rodar `capivaraos-upgrade`
    **hoje** normalmente responde que ainda não há destino publicado: é o
    comportamento correto, não um erro.

### Antes do upgrade: snapshot

Quando o disco suporta, a ferramenta tira um **snapshot antes** de começar, para
você poder voltar atrás:

- **btrfs + snapper**: snapshot **automático**. É o arranjo padrão do instalador
  nas três edições.
- **LVM**: não é automático — a ferramenta mostra o comando manual
  (`lvcreate -s …`) no pré-voo.
- **Sem storage compatível**: nada é fotografado — **faça backup** antes.

!!! tip "O pré-voo diz o que vai acontecer na sua máquina"
    Não precisa adivinhar em qual dos casos você está: rode `capivaraos-upgrade`
    sem argumento. Ele informa se vai tirar snapshot, e em que condições, antes
    de qualquer coisa ser baixada.

### Se precisar voltar atrás

O snapshot criado pela ferramenta aparece na lista com a descrição
**"pré capivaraos-upgrade"** — é o seu ponto de retorno:

```bash
sudo snapper list
```

Para desfazer as mudanças de arquivos daquele ponto até agora:

```bash
sudo snapper undochange <nº-do-snapshot>..0
```

O `0` significa "o estado atual". Para reverter **o sistema inteiro** depois de
um upgrade de geração, o caminho é **inicializar pelo snapshot e promovê-lo** —
o `undochange` desfaz mudanças de arquivos, não troca o sistema de lugar.

Em **LVM**, a reversão é `sudo lvconvert --merge <snapshot>` seguida de reinício.

## Por que a Série 1 não atualiza para a Série 2

A Série 1 chegou ao **fim de vida** e não tem o repositório assinado nem o motor
de upgrade in-place (ambos nasceram na Série 2). Não há, portanto, salto
in-place da Série 1 para a Série 2: quem está na Série 1 faz uma
**instalação limpa** da Série 2 — veja a [verificação do download](../instalacao/verificar-download.md)
e o [guia de instalação](../instalacao/guia.md). Guarde seus dados num backup
antes. A documentação da Série 1 segue disponível no
[Arquivo](../arquivo/1.x/index.md).
