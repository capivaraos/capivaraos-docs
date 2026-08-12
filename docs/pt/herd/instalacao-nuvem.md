# Implantação em nuvem / VM (qcow2)

A imagem **qcow2** já traz `cloud-init` e `qemu-guest-agent`, o que a torna ideal
para subir rápido em nuvem, KVM/libvirt, Proxmox e afins. O `cloud-init` cuida do
**usuário**, da **chave SSH**, do **hostname** e da **rede** no primeiro boot.

!!! info "Imagem = template"
    O qcow2 é um _template_. Cada VM criada a partir dele é personalizada pelo
    `cloud-init` na primeira inicialização (a partir do _datasource_).

## Opção A — KVM/libvirt com seed NoCloud

Sem um provedor de nuvem, o jeito mais simples de alimentar o `cloud-init` é um
_datasource_ **NoCloud**: um pequeno ISO com dois arquivos, `meta-data` e
`user-data`.

### 1. Crie os arquivos do seed

`meta-data`:

```yaml
instance-id: herd-01
local-hostname: herd-01
```

`user-data` (note o `#cloud-config` na primeira linha):

```yaml
#cloud-config
users:
  - name: admin
    groups: [wheel]
    sudo: "ALL=(ALL) NOPASSWD:ALL"
    shell: /bin/bash
    ssh_authorized_keys:
      - ssh-ed25519 AAAA... voce@maquina
# senha opcional — serve para CONSOLE e Cockpit (o SSH é só por chave)
chpasswd:
  expire: false
  users:
    - name: admin
      password: "troque-esta-senha"
      type: text
```

!!! warning "SSH é só por chave"
    O HERD vem com `PasswordAuthentication no`. **Inclua sua chave pública** em
    `ssh_authorized_keys`, senão você não entra por SSH. A senha (se definida)
    vale para o console e para o [Cockpit](cockpit.md).

### 2. Gere o ISO do seed

```bash
genisoimage -output seed.iso -volid cidata -joliet -rock user-data meta-data
# (ou: cloud-localds seed.iso user-data meta-data)
```

### 3. Suba a VM

```bash
cp CapivaraOS-HERD-1.0.0-x86_64.qcow2 herd-01.qcow2

virt-install --name herd-01 --memory 2048 --vcpus 2 \
  --disk path=herd-01.qcow2,format=qcow2,bus=virtio \
  --disk path=seed.iso,device=cdrom \
  --os-variant fedora-unknown --import \
  --network network=default,model=virtio --graphics none
```

Descubra o IP (`virsh domifaddr herd-01`) e entre: `ssh admin@<ip>`.

## Opção B — Provedor de nuvem

1. **Faça upload do qcow2** como imagem/template no seu provedor (OpenStack Glance,
   etc.) — ou importe como disco (Proxmox: `qm importdisk`).
2. Crie a instância a partir da imagem.
3. Forneça o **user-data** (mesmo `#cloud-config` acima) pelo mecanismo do
   provedor (metadata service / ConfigDrive). O `cloud-init` aplica no 1º boot.

!!! tip "qemu-guest-agent"
    Já vem instalado e habilitado — o host de virtualização consegue consultar
    IP, fazer _quiesce_ de snapshot e desligamento gracioso.

## Depois de subir

- [Primeiros passos](primeiros-passos.md) — atualizações, rede, fuso.
- [Console web (Cockpit)](cockpit.md) — `https://<ip>:9090`.
- [Segurança e compliance](seguranca.md).
