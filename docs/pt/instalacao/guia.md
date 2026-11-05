# Guia de instalação

## 1. Baixe a ISO

Acesse a [página de download](https://capivaraos.org/pt/download.html) e escolha a distro que quer instalar: [Marsh](../distros/marsh.md), [Pup](../distros/pup.md) ou [Snout](../distros/snout.md).

## 2. Confira o que você baixou

Antes de gravar, confira a soma SHA-256 da ISO. É rápido e evita o motivo mais comum de um pendrive que não inicia: download corrompido.

Se quiser ter certeza não só de que o arquivo chegou inteiro, mas de que ele é **nosso**, confira também a assinatura. O passo a passo está em [Verificar o download](verificar-download.md).

## 3. Grave no pendrive

Você vai precisar de um pendrive com pelo menos 4 GB. Recomendamos o **Fedora Media Writer** ou o **balenaEtcher** — eles gravam a imagem direto no pendrive e funcionam de primeira:

- **Fedora Media Writer** (Windows, macOS, Linux) — ferramenta oficial do Fedora, a mais indicada
- **balenaEtcher** (Windows, macOS, Linux) — simples, três cliques
- **dd** (Linux/macOS, via terminal):

```bash
sudo dd if=capivaraos-marsh-2.0.0.iso of=/dev/sdX bs=4M status=progress
```

!!! warning "Cuidado com o dd"
    Verifique qual é o dispositivo correto com `lsblk` antes de rodar o comando. Apagar o dispositivo errado é irreversível.

!!! note "Se o Ventoy travar na inicialização"
    O Ventoy guarda várias ISOs no mesmo pendrive e funciona na maioria dos casos, mas em alguns computadores a sessão live não sobe: o boot fica preso em *"Inicializando o CapivaraOS…"*, às vezes com uma mensagem parecida com `Can't create /lib/dracut/hooks/.../99-ventoy-….sh`. O Ventoy carrega a ISO por *loopback* e injeta esse script no boot — o erro é dele, não da imagem. Nesse caso, **grave a ISO direto no pendrive** com o Fedora Media Writer, o balenaEtcher ou o `dd`, e inicialize de novo.

## 4. Inicialize pelo pendrive

Reinicie o computador e entre no menu de boot (geralmente F12, F2 ou Del durante a inicialização). Selecione o pendrive.

No menu você verá **CapivaraOS \<edição\> 2 45** — o 2 é a versão do CapivaraOS e o 45 é a do Fedora que serve de base.

## 5. Teste ou instale

Ao iniciar, você verá a opção de entrar no modo live (testar sem instalar) ou iniciar a instalação diretamente.

!!! tip "Teste antes, se puder"
    O modo live roda da memória e não toca no seu disco. É a forma de confirmar que a rede sem fio, o som e o vídeo funcionam na sua máquina antes de instalar qualquer coisa.

## 6. Siga o instalador

O instalador do CapivaraOS é o Anaconda, o mesmo do Fedora. Ele vai guiar você pelas etapas:

1. **Idioma** — o sistema vem pronto em português do Brasil e em inglês; você escolhe na instalação e pode trocar depois pelo [COS Config Center](../apps/cos-config.md)
2. **Particionamento do disco** — o padrão é Btrfs, que é o que permite os instantâneos antes de cada atualização
3. **Criação do usuário**
4. **Instalação**

!!! note "Criptografar o disco"
    Se marcar a criptografia (LUKS), o computador vai pedir a senha do disco a cada boot, numa tela com a capivara. Guarde essa senha: sem ela não há como abrir o disco.

## 7. Primeiro boot

Remova o pendrive e reinicie. No primeiro login o **[COS Dev Center](../apps/cos-dev.md)** abre sozinho e pergunta o que você quer instalar — IA local, linguagens, ferramentas de desenvolvimento. Nada disso é obrigatório: dá para fechar e abrir depois pelo menu.

A partir daí o sistema se mantém atualizado sem reinstalar. Veja [Atualizar o sistema](../atualizar/index.md).

---

Encontrou algum problema? Posta no [fórum](https://capivaraos.org) ou abre uma issue no GitHub.
