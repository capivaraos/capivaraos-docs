# Segurança e compliance

O HERD já nasce **endurecido por padrão** — um servidor recém-instalado vem com
um perfil de segurança sensato, sem você precisar configurar nada. Esta página
explica **o que vem ativado**, como **auditar** e como **ajustar**.

## O que vem endurecido

| Área | Padrão no HERD |
|---|---|
| **SELinux** | `enforcing` |
| **Firewall** | `firewalld` liberando só **SSH** e **Cockpit** (mais `dhcpv6-client`); `mdns` desligado |
| **SSH** | só por **chave**; sem root; limites de tentativa (veja abaixo) |
| **Senhas** | qualidade mínima (`pwquality`) e idade de senha |
| **umask** | `027` em sessões de login (arquivos novos não ficam legíveis por "outros") |
| **Pacotes** | `gpgcheck=1` (verificação de assinatura no dnf) |

### SSH — política padrão

Arquivo: `/etc/ssh/sshd_config.d/50-capivaraos-herd.conf`

```text
PermitRootLogin no
PasswordAuthentication no
PermitEmptyPasswords no
KbdInteractiveAuthentication no
X11Forwarding no
MaxAuthTries 3
LoginGraceTime 30
AllowAgentForwarding no
```

### Política de senha

- Qualidade — `/etc/security/pwquality.conf.d/50-capivaraos-herd.conf`: tamanho
  mínimo **12**, ao menos **3 classes** de caracteres, sem repetições/sequências
  longas.
- Idade — `/etc/login.defs`: máximo **90** dias, mínimo **7** dias.

!!! note "Cifras (crypto policy)"
    Ciphers/MACs/KEX do SSH e TLS seguem a **crypto-policy do sistema** (já forte
    por padrão no Fedora), em vez de valores fixos no HERD.

## Auditar: `herd-compliance-scan`

O HERD embarca um scanner **OpenSCAP** com o conteúdo do **SCAP Security Guide**
do Fedora. Rode:

```bash
sudo herd-compliance-scan
```

Isso avalia o sistema contra o perfil **`standard`** e gera:

```
/root/herd-compliance-<data>.html   # relatório legível (abra no navegador)
/root/herd-compliance-<data>.xml    # resultados (máquina)
```

Para ver os perfis disponíveis ou rodar outro:

```bash
herd-compliance-scan --list
sudo herd-compliance-scan xccdf_org.ssgproject.content_profile_cis
```

!!! tip "Copiar o relatório para ver no seu PC"
    ```bash
    scp usuario@<ip>:/root/herd-compliance-*.html .
    ```
    (ou abra o arquivo pelo **Terminal**/**Arquivos** do Cockpit.)

!!! info "Perfil básico do Community"
    O HERD Community foca em um baseline sólido (SSH, firewall, SELinux, senha,
    umask). Controles mais pesados como **auditd completo**, **AIDE** e o perfil
    **CIS** integral são o caminho do **Enterprise**; o datastream embarcado já
    traz os perfis `cis`/`cis_server_l1` caso você queira avaliá-los.

## Ajustar

Toda a configuração vem do pacote `capivaraos-herd-hardening`. Você pode
sobrepor conforme a sua política.

### <a id="ssh"></a>SSH — habilitar senha (com ressalva)

!!! warning "Pense duas vezes"
    Autenticação por senha no SSH aumenta a superfície de ataque (força bruta).
    Prefira **chaves**. Só habilite senha se realmente precisar, e combine com
    `fail2ban`/limites de origem no firewall.

Crie um _drop-in_ que vem **depois** do nosso (ordem alfabética):

```bash
sudo tee /etc/ssh/sshd_config.d/60-local.conf >/dev/null <<'EOF'
PasswordAuthentication yes
EOF
sudo systemctl restart sshd
```

### Ajustar idade de senha

```bash
sudo tee /etc/security/pwquality.conf.d/60-local.conf >/dev/null <<'EOF'
minlen = 14
EOF
# idade: edite /etc/login.defs (PASS_MAX_DAYS / PASS_MIN_DAYS)
```

### Adicionar sua chave SSH

```bash
install -d -m 700 ~/.ssh
echo "ssh-ed25519 AAAA... voce@maquina" >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```

## Boas práticas para produção

- Exponha o mínimo: só as portas necessárias no `firewalld`.
- Acesse o Cockpit por VPN/túnel, não aberto na internet.
- Mantenha o sistema atualizado (`dnf upgrade`) — updates de segurança do Fedora.
- Rode o `herd-compliance-scan` periodicamente e trate o que fizer sentido para o
  seu ambiente.

## Créditos e licenças de terceiros

O `herd-compliance-scan` usa o **[SCAP Security Guide](https://github.com/ComplianceAsCode/content)**
(projeto ComplianceAsCode). O HERD redistribui o datastream do SSG para Fedora
(`ssg-fedora-ds.xml`), licenciado sob **BSD-3-Clause**:

> Copyright (c) 2012-2017, Red Hat, Inc. All rights reserved.
>
> A redistribuição e o uso, em código-fonte ou binário, com ou sem modificação,
> são permitidos desde que atendidas as condições da licença BSD-3-Clause
> (retenção do aviso de copyright, das condições e do *disclaimer*; sem uso do
> nome dos detentores para endosso). O texto integral acompanha o pacote
> `capivaraos-herd-hardening` em `/usr/share/licenses/capivaraos-herd-hardening/`
> e está disponível no [repositório do SSG](https://github.com/ComplianceAsCode/content/blob/master/LICENSE).

Os nomes de perfil (`standard`, `cis`, `cis_server_l1` etc.) vêm do próprio SSG.
"CIS" refere-se aos *benchmarks* do [Center for Internet Security](https://www.cisecurity.org/);
o HERD não reproduz o texto dos *benchmarks*, apenas executa os perfis
correspondentes disponibilizados pelo SSG.

---

*Conteúdo produzido com auxílio de IA, em revisão contínua. Encontrou um erro? [Abra uma issue](https://github.com/capivaraos/capivaraos-herd/issues).*
