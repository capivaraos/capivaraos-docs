# Administração

Tarefas do dia a dia de um servidor Herd, pelo terminal. Quase tudo também está
disponível no [Cockpit](cockpit.md), de forma gráfica.

## Atualizações

O Herd usa **dnf** (base Fedora):

```bash
sudo dnf upgrade --refresh          # tudo
sudo dnf upgrade --security         # só correções de segurança
sudo dnf check-update               # ver o que há de novo
```

Reinicie após atualizar o kernel: `sudo systemctl reboot`.

## Serviços (systemd)

```bash
systemctl status sshd               # estado de um serviço
sudo systemctl restart sshd         # reiniciar
sudo systemctl enable --now nginx   # habilitar no boot e iniciar agora
systemctl list-units --type=service --state=running
```

Logs de um serviço: `journalctl -u sshd`.

## Logs (journal)

```bash
journalctl -b                       # do boot atual
journalctl -p err -b                # só erros
journalctl -u <serviço> -f          # acompanhar em tempo real
journalctl --since "hoje"
```

## Armazenamento

```bash
lsblk                               # discos e partições
df -h                               # uso de sistemas de arquivos
sudo lvs; sudo vgs; sudo pvs        # LVM (a instalação padrão usa LVM)
```

Para adicionar/expandir discos, LVM e montagens, o módulo **Armazenamento** do
[Cockpit](cockpit.md) é o caminho mais simples.

## Rede

Gerenciada pelo NetworkManager (`nmcli`) — veja exemplos em
[Primeiros passos › Rede](primeiros-passos.md#rede).

```bash
nmcli device status
nmcli connection show
ip a                                # endereços atuais
```

## <a id="firewall"></a>Firewall (firewalld)

Por padrão a zona `public` libera **ssh**, **cockpit** e **dhcpv6-client**.

```bash
sudo firewall-cmd --list-all                                   # estado da zona
sudo firewall-cmd --permanent --add-service=https              # liberar um serviço
sudo firewall-cmd --permanent --add-port=8080/tcp             # liberar uma porta
sudo firewall-cmd --reload                                     # aplicar
```

Liberar uma porta só para uma rede de administração (mais seguro que abrir para
todos):

```bash
sudo firewall-cmd --permanent --zone=internal --add-source=192.168.10.0/24
sudo firewall-cmd --permanent --zone=internal --add-service=cockpit
sudo firewall-cmd --reload
```

## SELinux

Vem em `enforcing` (recomendado manter):

```bash
getenforce
sudo ausearch -m avc -ts recent     # ver negações recentes
sudo sealert -a /var/log/audit/audit.log   # análise (se setroubleshoot instalado)
```

!!! warning "Não desligue o SELinux"
    Se um serviço for barrado, prefira ajustar o contexto/boolean correto a
    desligar o SELinux. Desligar remove uma camada importante de contenção.

## Desligar / reiniciar

```bash
sudo systemctl reboot
sudo systemctl poweroff
```

## Próximos passos

- [Segurança e compliance](seguranca.md)
- [Suporte](suporte.md)

---

*Conteúdo produzido com auxílio de IA, em revisão contínua. Encontrou um erro? [Abra uma issue](https://github.com/capivaraos/capivaraos-herd/issues).*
