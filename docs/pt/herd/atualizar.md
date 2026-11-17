# Atualizar o Herd

O Herd 1.0.1 instalado recebe as atualizações do Fedora **sem formatar** e sem
esperar uma ISO nova. São dois tipos de atualização:

| Tipo | O que muda | Ferramenta |
|---|---|---|
| **Atualizações do dia a dia** | correções e novas versões de pacotes, dentro da mesma geração | `dnf upgrade` |
| **Upgrade de geração** | salto para a próxima geração do sistema (base Fedora nova) | não incluído no Herd 1.0.1 |

!!! note "Esta página é do Herd"
    As edições desktop usam o `capivaraos-upgrade` e contam com snapshot
    automático em Btrfs. No Herd as duas coisas são diferentes, e é disso que
    esta página trata. Para desktop, veja
    [Atualizar o sistema](../atualizar/index.md).

## Atualizações do dia a dia

```bash
sudo dnf upgrade --refresh
```

O Herd 1.0.1 só instala atualizações quando você roda esse comando; ele não se
atualiza sozinho. As atualizações vêm dos repositórios do Fedora, e o dnf
confere cada pacote dos repositórios fedora e updates contra as chaves de
assinatura do Fedora (`gpgcheck`) antes de instalar. Os pacotes do Herd nas
imagens 1.0.1 não são assinados e não têm repositório, então não são atualizados
depois da instalação.

## Upgrade de geração

O Herd 1.0.1 não inclui o `herd-upgrade`, e não há repositório do Herd para uma
geração nova.

## Snapshot e recuperação

Esta é a parte que vale planejar antes.

O layout usual de servidor é **LVM + xfs**, e nesse arranjo **não há snapshot
automático**: o Snapper cobre Btrfs, não xfs. O Herd também **não vem com o
Snapper instalado e configurado**. O caso depende da sua instalação:

- **raiz em LVM** — snapshot automático não é feito. Um snapshot manual
  (`lvcreate -s …`) depende de **haver espaço livre no
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
