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
    Após entrar, clique em **"Ativar acesso administrativo"** (canto superior) e
    informe a senha novamente. Isso libera as ações que exigem privilégio
    (instalar updates, mexer em serviços, storage, etc.).

## O que dá para fazer

| Módulo | Para quê |
|---|---|
| **Visão geral** | Saúde (CPU, memória), identidade do sistema, hora, hostname |
| **Logs** | Navegar o journal (filtrar por prioridade, serviço, período) |
| **Armazenamento** | Discos, partições, LVM, sistemas de arquivos, montagens |
| **Rede** | Interfaces, IP, firewall, _bonds_/bridges/VLANs |
| **Contas** | Criar/editar usuários, grupos, chaves SSH, expiração de senha |
| **Serviços** | Habilitar/parar/reiniciar unidades systemd, ver status |
| **Atualizações** | Aplicar updates de segurança e do sistema |
| **Terminal** | Shell do servidor direto no navegador |

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
