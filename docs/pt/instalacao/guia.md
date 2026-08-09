# Guia de instalação

## 1. Baixe a ISO

Acesse a [página de download](https://capivaraos.org/pt/download.html) e escolha a distro que quer instalar: Marsh, Pup ou Snout.

## 2. Grave no pendrive

Você vai precisar de um pendrive com pelo menos 4 GB. Use uma das ferramentas abaixo:

- **Balena Etcher** (Windows, macOS, Linux) — mais fácil
- **Ventoy** (Windows, Linux) — permite múltiplas ISOs no mesmo pendrive
- **dd** (Linux/macOS, via terminal):

```bash
sudo dd if=capivaraos-marsh-1.x.x.iso of=/dev/sdX bs=4M status=progress
```

!!! warning "Cuidado com o dd"
    Verifique qual é o dispositivo correto com `lsblk` antes de rodar o comando. Apagar o dispositivo errado é irreversível.

## 3. Inicialize pelo pendrive

Reinicie o computador e entre no menu de boot (geralmente F12, F2 ou Del durante a inicialização). Selecione o pendrive.

## 4. Teste ou instale

Ao iniciar, você verá a opção de entrar no modo live (testar sem instalar) ou iniciar a instalação diretamente.

## 5. Siga o Anaconda

O instalador do CapivaraOS é o Anaconda, o mesmo do Fedora. Ele vai guiar você pelas etapas:

1. Idioma (já vem pré-configurado como Português do Brasil)
2. Particionamento do disco
3. Criação do usuário
4. Instalação

## 6. Pronto

Após a instalação, remova o pendrive e reinicie. O sistema já vem configurado em português, com o ambiente gráfico pronto para uso.

---

Encontrou algum problema? Posta no [fórum](https://capivaraos.org) ou abre uma issue no GitHub.
