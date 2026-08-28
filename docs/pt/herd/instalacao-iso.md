# Instalação via ISO (bare-metal)

Este guia cobre a instalação do Herd Community em um servidor **físico** ou em
uma **VM**, usando a ISO instaladora. O instalador é o **Anaconda em modo texto**
— ideal para servidores _headless_, sem depender de interface gráfica.

!!! info "Instalação offline"
    A ISO é **autossuficiente**: os pacotes vêm embutidos, então a instalação
    **não precisa de internet**.

## 1. Prepare a mídia de boot

Grave a ISO em um pendrive (≥ 2 GB) ou monte-a como mídia virtual (iDRAC/iLO/IPMI,
ou "CD/DVD" da VM).

=== "Linux/macOS (dd)"

    ```bash
    lsblk   # descubra o dispositivo do pendrive (ex.: /dev/sdX)
    sudo dd if=CapivaraOS-HERD-1.0.0-x86_64.installer.iso of=/dev/sdX bs=4M status=progress oflag=sync
    ```

    !!! warning "Confirme o dispositivo"
        `dd` no disco errado apaga dados de forma irreversível. Confira com
        `lsblk` antes.

=== "Windows"

    Use o **Balena Etcher** ou o **Rufus** (modo DD/imagem).

=== "VM"

    Aponte o drive de CD/DVD da VM para o arquivo `.iso`. Não precisa gravar
    pendrive.

## 2. Dê boot pela ISO

Entre no menu de boot do servidor (geralmente **F11/F12/Del/Esc**) e selecione a
mídia. O menu do GRUB do CapivaraOS aparece — escolha **Install Herd by CapivaraOS**.

O Anaconda inicia em **modo texto** e mostra um menu numerado. Cada item entre
`[x]` já está configurado; os `[ ]` precisam da sua atenção.

## 3. Configure os itens obrigatórios

O perfil já vem pronto (idioma pt_BR + en_US, teclado ABNT2, fuso
America/Sao_Paulo, rede por DHCP, fonte de instalação offline). Você precisa
definir:

### Disco (destino da instalação)

Escolha o número de **"Destino da instalação"**, selecione o disco e confirme o
particionamento automático (ou defina o seu). 

!!! tip "Particionamento"
    O automático usa LVM e serve para a maioria dos casos. Para layouts
    específicos (ex.: `/var` separado, tamanhos de cota), configure manualmente.

### Senha do root

Defina em **"Senha do root"**. O login direto de root por SSH fica **desabilitado**
por padrão (veja [Segurança](seguranca.md)); a senha de root serve para o
**console** (físico/IPMI) e recuperação.

### Crie um usuário administrador

Este passo é importante — é a conta que você vai usar no dia a dia e no
[Cockpit](cockpit.md) (o Cockpit **bloqueia login de root**).

1. Escolha o número de **"Criação de usuários"**.
2. Digite `1` para **criar o usuário** — o formulário expande.
3. Preencha **nome de usuário** e **senha**.
4. Marque **Administrador** (`[x]`) — isso coloca a conta no grupo `wheel`
   (permite `sudo` e habilitar acesso administrativo no Cockpit).
5. Digite `c` para continuar.

## 4. Inicie a instalação

De volta ao menu principal, com todos os itens em `[x]`, digite `b` (_begin_)
para instalar. Ao terminar, **remova a mídia** e reinicie.

## 5. Primeiro boot

No menu do GRUB você verá a entrada **Herd**. O sistema sobe em modo
texto e mostra, no console, o endereço do Cockpit:

```
Herd Community 1.0.0  ·  servidor headless (Fedora 44)
Web console: https://<ip>:9090/
```

## 6. Acesso remoto — leia isto

Por padrão o SSH aceita **apenas chave** (`PasswordAuthentication no`). Ou seja,
logo após instalar **você não consegue entrar por SSH com senha**. Opções:

- **Console primeiro (recomendado):** logue no console físico/IPMI com o usuário
  admin e adicione sua chave pública:
  ```bash
  install -d -m 700 ~/.ssh
  echo "ssh-ed25519 AAAA... voce@maquina" >> ~/.ssh/authorized_keys
  chmod 600 ~/.ssh/authorized_keys
  ```
  Depois: `ssh usuario@<ip>`.
- **Cockpit:** o console web em `https://<ip>:9090` aceita **usuário + senha**
  (não é SSH) — dá para administrar sem chave. Veja [Console web](cockpit.md).
- **Precisa mesmo de SSH por senha?** É possível relaxar a política, com ressalvas
  de segurança — veja [Segurança › Ajustar o SSH](seguranca.md#ssh).

## Próximos passos

- [Primeiros passos](primeiros-passos.md) — rede, hostname, atualizações.
- [Console web (Cockpit)](cockpit.md) — administrar pelo navegador.
- [Segurança e compliance](seguranca.md) — o que vem endurecido.

---

*Conteúdo produzido com auxílio de IA, em revisão contínua. Encontrou um erro? [Abra uma issue](https://github.com/capivaraos/capivaraos-herd/issues).*
