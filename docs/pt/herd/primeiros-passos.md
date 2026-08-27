# Primeiros passos

Depois de instalar, estes são os ajustes iniciais de um servidor Herd. Tudo aqui
pode ser feito pelo terminal (SSH/console) ou pelo [Cockpit](cockpit.md).

## Acesso

- **SSH (por chave):** `ssh usuario@<ip>` — lembre que o SSH é **só por chave**
  (veja [Instalação via ISO › Acesso remoto](instalacao-iso.md#6-acesso-remoto-leia-isto)).
- **Console:** físico/IPMI, com o usuário admin ou root.
- **Cockpit:** `https://<ip>:9090` (usuário + senha).

## Elevar privilégios (sudo)

O usuário administrador (grupo `wheel`) usa `sudo`:

```bash
sudo dnf upgrade --refresh
```

## Atualize o sistema

Antes de tudo, deixe o sistema em dia:

```bash
sudo dnf upgrade --refresh
sudo systemctl reboot   # se um kernel novo foi instalado
```

## Hostname

```bash
sudo hostnamectl set-hostname herd-01.exemplo.com.br
```

## Rede

A rede vem por **DHCP**. Para IP fixo, use o NetworkManager (`nmcli`):

```bash
# ver conexões e a interface
nmcli connection show
nmcli device status

# definir IP estático (ajuste nome da conexão e valores)
sudo nmcli connection modify "System eth0" \
  ipv4.method manual \
  ipv4.addresses 192.168.0.10/24 \
  ipv4.gateway 192.168.0.1 \
  ipv4.dns "1.1.1.1 8.8.8.8"

sudo nmcli connection up "System eth0"
```

!!! tip "Pelo Cockpit"
    Dá para fazer o mesmo em **Rede** no [Cockpit](cockpit.md), com interface
    gráfica.

## Fuso horário e hora

```bash
timedatectl                              # estado atual
sudo timedatectl set-timezone America/Sao_Paulo
```

A sincronização de hora (`chronyd`) já vem ativa. Na imagem **qcow2** ela usa o
`pool.ntp.br`; na instalação por **ISO**, os servidores padrão do Fedora (ajuste
em `/etc/chrony.conf` se quiser).

## Idioma / locale

O sistema traz `pt_BR.UTF-8` e `en_US.UTF-8`. Para mudar o padrão:

```bash
localectl                                # estado atual
sudo localectl set-locale LANG=pt_BR.UTF-8
```

## Usuários e chaves SSH

Criar outro administrador:

```bash
sudo useradd -m -G wheel maria
sudo passwd maria
# adicione a chave pública dela:
sudo install -d -m 700 /home/maria/.ssh
echo "ssh-ed25519 AAAA... maria@pc" | sudo tee -a /home/maria/.ssh/authorized_keys
sudo chown -R maria:maria /home/maria/.ssh
sudo chmod 600 /home/maria/.ssh/authorized_keys
```

Também dá para gerenciar contas em **Contas** no [Cockpit](cockpit.md).

## Libere as portas das suas aplicações

O `firewalld` só libera **SSH** e **Cockpit** por padrão. Para um serviço web,
por exemplo:

```bash
sudo firewall-cmd --permanent --add-service=http --add-service=https
sudo firewall-cmd --reload
sudo firewall-cmd --list-all
```

Veja mais em [Administração › Firewall](administracao.md#firewall).

## Próximos passos

- [Console web (Cockpit)](cockpit.md)
- [Segurança e compliance](seguranca.md) — rode o `herd-compliance-scan`.
- [Administração](administracao.md) — serviços, storage, logs, updates.

---

*Conteúdo produzido com auxílio de IA, em revisão contínua. Encontrou um erro? [Abra uma issue](https://github.com/capivaraos/capivaraos-herd/issues).*
