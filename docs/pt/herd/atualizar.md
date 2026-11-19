# Atualizar o Herd

O Herd instalado recebe atualizações **sem formatar** e sem esperar uma ISO
nova. São dois tipos de atualização:

| Tipo | O que muda | Ferramenta |
|---|---|---|
| **Atualizações do dia a dia** | correções e novas versões de pacotes, dentro da mesma geração | atualizações automáticas de segurança, mais o `dnf upgrade` |
| **Upgrade de geração** | salto para a próxima geração do sistema (base Fedora nova) | `herd-upgrade` |

!!! note "Esta página é do Herd"
    As edições desktop se atualizam com outra ferramenta. Esta página trata só
    do Herd. Para desktop, veja
    [Atualizar o sistema](../atualizar/index.md).

!!! tip "Vindo do Herd 1.0.1?"
    O Herd 1.0.1 não tem repositório do Herd nem atualizações automáticas, então
    não passa à 2.0 sozinho.

## De onde vêm as atualizações

- **Repositórios do Fedora** (`fedora` e `updates`), para o sistema base.
- **Repositório do Herd** (`capivaraos-herd`), para os pacotes do Herd:
  identidade visual, fortalecimento, a política de atualização e as ferramentas
  desta página.

O dnf confere cada pacote contra a chave de assinatura antes de instalar. No
repositório do Herd ele também confere a assinatura do índice do repositório,
então uma lista de pacotes adulterada também é recusada. Como conferir isso
você mesmo está em [Verificar o software](seguranca.md#verificar-o-software).

## Atualizações automáticas de segurança

O Herd instala as **atualizações de segurança sozinho**, uma vez por dia, e não
reinicia o servidor. Quem faz isso é o `dnf5-automatic.timer`, ligado desde o
primeiro boot. A política fica em `/etc/dnf/automatic.conf`:

| Opção | Valor | Significado |
|---|---|---|
| `upgrade_type` | `security` | só atualizações marcadas como correção de segurança |
| `apply_updates` | `yes` | baixa e instala |
| `reboot` | `never` | o servidor continua no ar; você escolhe quando reiniciar |

Atualizações que não são marcadas como correção de segurança chegam com o
`dnf upgrade` (próxima seção).

Confira se está ligado e veja a última execução:

```bash
systemctl is-active dnf5-automatic.timer
journalctl -u dnf5-automatic.service -n 30
```

O resultado de cada execução também aparece na mensagem de login.

O arquivo é seu para editar. Para desligar as atualizações automáticas:

```bash
sudo systemctl disable --now dnf5-automatic.timer
```

!!! note "Reinicie depois de atualizar o kernel"
    Um kernel novo, ou uma versão nova de uma biblioteca usada por um serviço
    em execução, só vale depois de reiniciar. Planeje os reinícios na sua
    janela de manutenção: `sudo systemctl reboot`.

!!! warning "O cartão de atualizações automáticas no Cockpit"
    A página **Atualizações de Software** do Cockpit mostra **Atualizações
    automáticas: Desabilitado** no Herd, mesmo com o `dnf5-automatic.timer`
    ativo. O cartão não mostra a política do Herd. Não mude a opção por
    esse cartão; use os comandos acima.

## Atualizações do dia a dia

Para instalar todas as atualizações disponíveis, não só as de segurança:

```bash
sudo dnf upgrade --refresh
```

Isso cobre os repositórios do Fedora e do Herd. A página **Atualizações de
Software** do Cockpit lista e aplica as atualizações disponíveis pelo navegador.

## Upgrade de geração

O `herd-upgrade` verifica se o Herd pode passar para uma base Fedora mais nova.

```bash
herd-upgrade                  # só verifica: não muda nada
sudo herd-upgrade download    # baixa o upgrade; o servidor continua no ar
```

Por padrão, o `herd-upgrade` se recusa a baixar enquanto o repositório do Herd
não tiver pacotes para a versão do Fedora de destino. O Herd 2.0 não tem uma
geração mais nova para onde subir.

## Snapshot e recuperação

Esta é a parte que vale planejar antes.

O layout usual de servidor é **LVM + xfs**, e nesse arranjo **não há snapshot
automático**: o Snapper cobre Btrfs, não xfs. O Herd também **não vem com o
Snapper instalado e configurado**. O caso depende da sua instalação:

- **raiz em LVM** — snapshot automático não é feito. Um snapshot manual
  (`lvcreate -s …`) depende de **haver espaço livre no
  grupo de volumes**. Numa instalação em que a raiz cresceu até ocupar o disco,
  não há espaço de onde tirar o snapshot.
- **raiz em Btrfs** — um snapshot com o Snapper só é possível se você mesmo
  instalou e configurou o Snapper.
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
- [Console web (Cockpit)](cockpit.md)
