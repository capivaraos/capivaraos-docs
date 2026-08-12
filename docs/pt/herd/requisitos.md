# Requisitos

O HERD Community 1.0.0 é distribuído para **x86_64** (64 bits). O suporte a
**aarch64** (ARM) chega na versão 1.1.

## Hardware mínimo e recomendado

| Componente | Mínimo | Recomendado |
|---|---|---|
| **Arquitetura** | x86_64 (64 bits) | x86_64 |
| **CPU** | 1 núcleo | 2+ núcleos |
| **RAM** | 1 GB | 2 GB ou mais |
| **Disco** | 10 GB | 20 GB ou mais |
| **Rede** | 1 interface Ethernet | — |
| **Firmware** | BIOS legado **ou** UEFI | UEFI |

!!! note "Servidor real x teste"
    Os mínimos acima bastam para subir e administrar o sistema. Para cargas de
    produção (bancos de dados, containers, etc.), dimensione CPU/RAM/disco
    conforme a aplicação.

## Firmware e boot

- A ISO instaladora funciona tanto em **UEFI** quanto em **BIOS legado**.
- Em UEFI com **Secure Boot**, o HERD usa o `shim` assinado do Fedora — o boot
  funciona sem desativar o Secure Boot na maioria dos equipamentos.

## Virtualização e nuvem

O HERD roda bem em virtualização e nuvem. A imagem **qcow2** já traz
`qemu-guest-agent` e `cloud-init`:

| Plataforma | Observação |
|---|---|
| **KVM/QEMU, libvirt** | Use a imagem `qcow2` diretamente |
| **Proxmox VE** | Importe o `qcow2` como disco da VM |
| **VMware / VirtualBox** | Use a ISO instaladora, ou converta o `qcow2` |
| **Nuvem (OpenStack, etc.)** | `qcow2` + `cloud-init` (datasource NoCloud/ConfigDrive) |

!!! tip "Firewall já vem restritivo"
    Por padrão o `firewalld` libera apenas **SSH (22)** e **Cockpit (9090)**.
    Planeje liberar as portas das suas aplicações — veja
    [Administração › Firewall](administracao.md#firewall).

## Rede

- Por padrão a rede é configurada por **DHCP**.
- IP fixo pode ser definido na instalação (Anaconda), depois via
  [`nmcli`](primeiros-passos.md#rede) ou pelo [Cockpit](cockpit.md).
- IPv6 via DHCPv6 é suportado (o serviço `dhcpv6-client` fica liberado no
  firewall de propósito).

---

*Conteúdo produzido com auxílio de IA, em revisão contínua. Encontrou um erro? [Abra uma issue](https://github.com/capivaraos/capivaraos-herd/issues).*
