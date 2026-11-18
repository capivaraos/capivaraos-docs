# Console web (Cockpit)

O Herd já vem com o **Cockpit**, um painel de administração no navegador. Dá para
ver saúde do sistema, gerenciar serviços, rede, armazenamento, contas, ler logs e
até abrir um terminal — sem instalar nada a mais.

## Acesso

Abra no navegador:

```
https://<ip-do-servidor>:9090
```

!!! note "Certificado autoassinado"
    Na primeira vez o navegador avisa sobre o certificado (autoassinado). É
    esperado num servidor recém-instalado — prossiga. Para produção, você pode
    instalar um certificado próprio em `/etc/cockpit/ws-certs.d/`.

O serviço é ativado **sob demanda** (`cockpit.socket`): ele só sobe quando alguém
acessa a porta 9090, que já vem liberada no firewall.

## Login

- Use um **usuário do sistema** com sua **senha** (o mesmo login do console).
- O **root é bloqueado** no Cockpit por padrão — use o usuário administrador
  (grupo `wheel`) que você criou.

!!! tip "Acesso administrativo"
    Após entrar, o Cockpit começa em **Acesso limitado**. Clique em
    **Acesso limitado** (canto superior) e informe a sua senha de novo para
    passar ao **Acesso administrativo**. O menu é o mesmo nos dois modos; o modo
    administrativo libera as ações que exigem privilégio (instalar updates,
    mexer em serviços, storage, etc.).

## O que dá para fazer

| Módulo | Para quê |
|---|---|
| **Visão geral** | Saúde (CPU, memória), identidade do sistema, hora, hostname |
| **Logs** | Navegar o journal (filtrar por prioridade, serviço, período) |
| **Armazenamento** | Discos, partições, LVM, sistemas de arquivos, montagens |
| **Rede** | Interfaces, IP, firewall, _bonds_/bridges/VLANs |
| **Contas** | Criar/editar usuários, grupos, chaves SSH, expiração de senha |
| **Serviços** | Habilitar/parar/reiniciar unidades systemd, ver status |
| **Atualizações de Software** | Ver e aplicar as atualizações disponíveis (veja o aviso abaixo) |
| **Terminal** | Shell do servidor direto no navegador |

!!! warning "Cartão de atualizações automáticas"
    A página **Atualizações de Software** mostra **Atualizações automáticas:
    Desabilitado** no Herd, mesmo com o `dnf5-automatic.timer` ativo. O cartão
    não mostra a política do Herd: o Herd instala as atualizações de segurança
    automaticamente todo dia. Não mude a opção por
    esse cartão. Veja
    [Atualizar o Herd](atualizar.md#atualizacoes-automaticas-de-seguranca).

## Identidade CapivaraOS

O Cockpit do Herd vem com a marca do CapivaraOS (tela de login e cabeçalho) e
**sem** links de documentação apontando para terceiros — o link de docs leva a
esta documentação.

## Dica de segurança

O Cockpit é uma porta de administração poderosa. Em servidores expostos à
internet, considere:

- Acessá-lo por **VPN** ou **túnel SSH** em vez de expor a 9090 publicamente:
  ```bash
  ssh -L 9090:localhost:9090 usuario@<ip>
  # depois abra https://localhost:9090 no seu PC
  ```
- Restringir a origem no `firewalld` (liberar a 9090 só para a rede de
  administração).

Veja [Segurança e compliance](seguranca.md).

---

*Conteúdo produzido com auxílio de IA, em revisão contínua. Encontrou um erro? [Abra uma issue](https://github.com/capivaraos/capivaraos-herd/issues).*
