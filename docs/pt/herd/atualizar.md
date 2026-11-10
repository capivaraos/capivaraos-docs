# Atualizar o Herd

O Herd instalado se mantém atualizado **sem formatar** e sem esperar uma ISO
nova, pelo mesmo repositório assinado das outras edições. São dois tipos de
atualização, com ferramentas diferentes:

| Tipo | O que muda | Ferramenta |
|---|---|---|
| **Atualizações do dia a dia** | correções e novas versões de pacotes, dentro da mesma geração | `dnf upgrade` |
| **Upgrade de geração** | salto para a próxima geração do sistema (base Fedora nova) | `herd-upgrade` |

!!! note "Esta página é do Herd"
    As edições desktop usam o `capivaraos-upgrade` e contam com snapshot
    automático em Btrfs. No Herd as duas coisas são diferentes, e é disso que
    esta página trata. Para desktop, veja
    [Atualizar o sistema](../atualizar/index.md).

## Atualizações do dia a dia

```bash
sudo dnf upgrade --refresh
```

Além dos pacotes do Fedora, você recebe os **nossos** a partir de um
**repositório assinado**: cada pacote **e** os metadados são verificados contra
a nossa chave GPG (`gpgcheck` + `repo_gpgcheck`). Assinatura que não confere
**interrompe a transação** — não há caminho para instalar um pacote nosso que
não passe pela verificação.

## Upgrade de geração

O `herd-upgrade` usa o `dnf5 system-upgrade` e trabalha em passos explícitos:

```bash
herd-upgrade                   # pré-voo: mostra o que aconteceria; não altera nada
sudo herd-upgrade download     # baixa a transação offline (não reinicia)
sudo herd-upgrade reboot       # aplica agora, reiniciando no modo offline
```

Sem argumento ele roda o pré-voo e **nada é alterado**. O destino padrão é a
geração atual + 1; para outra, use `--to N`.

!!! tip "Rode o pré-voo primeiro"
    O pré-voo responde, para a **sua** máquina, se já existe destino publicado,
    qual é o sistema de arquivos da raiz e se um snapshot vai ser tirado. Não
    precisa adivinhar em qual caso você está.

Se ainda não houver geração de destino publicada, o pré-voo **recusa** — do
contrário a máquina subiria uma base Fedora nova e deixaria os pacotes do
CapivaraOS para trás. Isso é o comportamento correto, não um erro.

## Snapshot e recuperação

Esta é a parte que vale planejar antes.

O layout usual de servidor é **LVM + xfs**, e nesse arranjo **não há snapshot
automático**: o Snapper cobre Btrfs, não xfs. O Herd também **não vem com o
Snapper instalado e configurado**. O `herd-upgrade` detecta o caso e diz no
pré-voo em qual você está:

- **raiz em LVM** — snapshot automático não é feito. A ferramenta mostra o
  comando manual (`lvcreate -s …`), que depende de **haver espaço livre no
  grupo de volumes**. Numa instalação em que a raiz cresceu até ocupar o disco,
  não há espaço de onde tirar o snapshot.
- **raiz em Btrfs com uma configuração `root` do Snapper ativa** — snapshot
  automático, como nas edições desktop. Isso exige que você mesmo tenha
  instalado e configurado o Snapper.
- **nenhum dos dois** — nada é fotografado.

Em servidor, portanto, **o caminho de recuperação é o backup**. Faça um backup
verificado antes de um upgrade de geração e trate a restauração como o plano de
volta.

!!! warning "Não conte com reversão por snapshot no Herd"
    A reversão por snapshot — e também o `lvconvert --merge` — só funciona se o
    snapshot existir. No layout padrão do Herd ele não existe. Planeje com
    backup.

### Kernel que não sobe é outro caso

Um kernel novo que não inicializa não exige restaurar nada. O sistema mantém as
**três últimas** versões de kernel instaladas (padrão do dnf) e o menu do GRUB
permite inicializar pela anterior. Isso vale para a troca de kernel, não para um
upgrade de geração.

## Links

- [Segurança e compliance](seguranca.md)
- [Administração](administracao.md)
- [Verificação do download](download.md)

---

*Conteúdo produzido com auxílio de IA, em revisão contínua. Encontrou um erro? [Abra uma issue](https://github.com/capivaraos/capivaraos-herd/issues).*
