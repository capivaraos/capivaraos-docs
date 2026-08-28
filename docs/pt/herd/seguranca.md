# Segurança e compliance

O Herd já nasce **endurecido por padrão** — um servidor recém-instalado vem com
um perfil de segurança sensato, sem você precisar configurar nada. Esta página
explica **o que vem ativado**, como **auditar** e como **ajustar**.

## O que vem endurecido

| Área | Padrão no Herd |
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
    por padrão no Fedora), em vez de valores fixos no Herd.

## Auditar: `herd-compliance-scan`

O Herd embarca um scanner **OpenSCAP** com o conteúdo do **SCAP Security Guide**
do Fedora. Rode:

```bash
sudo herd-compliance-scan
```

Isso avalia o sistema contra o perfil **`standard`** e gera:

```
/root/herd-compliance-<data>.html      # relatório legível (abra no navegador)
/root/herd-compliance-<data>.xml       # resultados XCCDF (máquina)
/root/herd-compliance-<data>.arf.xml   # ARF: evidência re-renderizável / auditoria
```

Para ver os perfis disponíveis ou rodar outro (aceita os **mesmos apelidos** do
[`herd-harden`](endurecimento.md) — `standard`, `ospp`, `cis-l1`, `cis-l2`, `pci`
— ou o id completo):

```bash
herd-compliance-scan --list
sudo herd-compliance-scan ospp
```

!!! tip "ARF é a evidência"
    O `.arf.xml` é o formato *machine-readable* que guarda datastream + resultados
    juntos: dá para **re-gerar o relatório depois** sem escanear de novo e é o que
    ferramentas de auditoria/agregação consomem.

!!! tip "Copiar o relatório para ver no seu PC"
    ```bash
    scp usuario@<ip>:/root/herd-compliance-*.html .
    ```
    (ou abra o arquivo pelo **Terminal**/**Arquivos** do Cockpit.)

!!! info "Perfil básico do Community"
    O Herd Community foca em um baseline sólido (SSH, firewall, SELinux, senha,
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

!!! warning "Vai aplicar `ospp`/`cis` ou o modo FIPS? Não dependa de ed25519"
    Esses perfis trocam a *crypto-policy* do sistema para **FIPS**, que **não
    aceita chaves ed25519** (nem cifras fora do conjunto FIPS). Gere uma chave
    **RSA** (`ssh-keygen -t rsa -b 3072`) ou **ECDSA** (`ssh-keygen -t ecdsa -b 384`)
    **antes** de endurecer e confirme um login novo. Para o baseline padrão, a
    ed25519 continua ótima. Veja [Endurecimento em 1 comando](endurecimento.md).

## Boas práticas para produção

- Exponha o mínimo: só as portas necessárias no `firewalld`.
- Acesse o Cockpit por VPN/túnel, não aberto na internet.
- Mantenha o sistema atualizado (`dnf upgrade`) — updates de segurança do Fedora.
- Rode o `herd-compliance-scan` periodicamente e trate o que fizer sentido para o
  seu ambiente.

## Criptografia em repouso e FIPS (opcional)

Dois reforços que ficam **desligados por padrão** (para não travar servidor
headless e nuvem) e você habilita quando a sua política pedir.

### Criptografia de disco (LUKS)

Protege os dados **em repouso** (disco roubado/descartado não vaza nada). É uma
decisão de **instalação**:

- **Recomendado:** na tela de particionamento do instalador, marque
  **"Criptografar meus dados"** e defina a senha.
- **Automatizado:** o [kickstart do Herd](instalacao-iso.md) traz um template
  comentado (`autopart --encrypted`) para instalação já cifrada.

Conferir depois:

```bash
lsblk                       # o dispositivo deve aparecer como "crypt"
sudo cryptsetup status luks-<uuid>
```

!!! warning "Servidor headless: a senha é pedida no boot"
    Um volume LUKS pede a **senha a cada boot**. Sem console (físico/IPMI/serial),
    a máquina **não sobe sozinha** depois de reiniciar. Para auto-desbloqueio em
    servidor, o caminho é **NBDE** — `clevis` com **TPM2** (selo local) ou
    **Tang** (chave pela rede). **Não** use LUKS na imagem de **nuvem** (qcow2).

### Modo FIPS

O FIPS 140 força o uso apenas de módulos criptográficos validados e restringe os
algoritmos permitidos (via *crypto-policy* `FIPS`). Útil para requisitos
governamentais/setoriais.

Habilitar (exige reinício):

```bash
sudo fips-mode-setup --enable
sudo reboot
```

Conferir:

```bash
fips-mode-setup --check
cat /proc/sys/crypto/fips_enabled     # 1 = ativo
```

Para desativar: `sudo fips-mode-setup --disable && sudo reboot`.

!!! warning "Teste o acesso antes de depender disso"
    O modo FIPS **restringe algoritmos**: em especial, **chaves SSH ed25519
    deixam de ser aceitas** (use **RSA** ou **ECDSA**), além de cifras e
    certificados fora do conjunto aprovado. **Confirme o login SSH numa segunda
    sessão** logo após reiniciar, antes de fechar o acesso que você tem.

## Créditos e licenças de terceiros

O `herd-compliance-scan` usa o **[SCAP Security Guide](https://github.com/ComplianceAsCode/content)**
(projeto ComplianceAsCode). O Herd redistribui o datastream do SSG para Fedora
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
o Herd não reproduz o texto dos *benchmarks*, apenas executa os perfis
correspondentes disponibilizados pelo SSG.

---

*Conteúdo produzido com auxílio de IA, em revisão contínua. Encontrou um erro? [Abra uma issue](https://github.com/capivaraos/capivaraos-herd/issues).*
