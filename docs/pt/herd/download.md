# Download e verificação

## Escolha a imagem

O Herd Community 2.0.0 é distribuído em duas imagens para **x86_64**:

| Imagem | Arquivo | Quando usar |
|---|---|---|
| **ISO instaladora** | `CapivaraOS-HERD-2.0.0-x86_64.installer.iso` | Instalar em servidor físico ou VM a partir do zero |
| **Disco qcow2** | `CapivaraOS-HERD-2.0.0-x86_64.qcow2` | Subir rápido em nuvem/virtualização com `cloud-init` |

Baixe em:

- **Site:** [capivaraos.org — Download](https://capivaraos.org/pt/download.html)
- **SourceForge:** [pasta CapivaraOS-HERD/2.0.0](https://sourceforge.net/projects/capivaraos/files/CapivaraOS-HERD/2.0.0/)

A mesma pasta tem o `SHA256SUMS`, com os checksums das duas imagens, e o
`SHA256SUMS.asc`, a assinatura dele. Baixe os dois junto com a imagem.

## 1. O arquivo chegou inteiro? (checksum)

```
0fac4847ccb6950803aa6c18a9d8daaacbea1f2b0ce728ed532945b825bbe375  CapivaraOS-HERD-2.0.0-x86_64.qcow2
c0934b5fe32999d422c4f14002b94f1c08d54456d83c441b9b6ad1597ff97ac9  CapivaraOS-HERD-2.0.0-x86_64.installer.iso
```

=== "Linux / macOS"

    ```bash
    # com o SHA256SUMS na mesma pasta da imagem, rode:
    sha256sum -c --ignore-missing SHA256SUMS
    # deve responder "OK" para o(s) arquivo(s) que você baixou
    ```

=== "Windows (PowerShell)"

    ```powershell
    Get-FileHash .\CapivaraOS-HERD-2.0.0-x86_64.installer.iso -Algorithm SHA256
    # compare a saída com o valor acima
    ```

    Isso confere que o arquivo chegou inteiro. Para conferir também que ele é
    nosso (passo 2), use o GnuPG para Windows (Gpg4win) ou uma máquina Linux.

!!! warning "Se o checksum não bater"
    Não use a imagem. Um hash diferente indica download corrompido ou arquivo
    adulterado — baixe de novo, de preferência da fonte oficial.

## 2. O arquivo é nosso? (assinatura)

O checksum prova que o arquivo chegou inteiro, mas não **quem** o publicou:
quem trocasse a imagem publicaria um checksum que bate também. A assinatura
responde isso. O `SHA256SUMS.asc` é a assinatura GPG do `SHA256SUMS`, feita com
a chave do Herd, a mesma que assina as atualizações do Herd.

**a) Baixe a chave pública do Herd e confira:**

```bash
curl -fsSLO https://repo-herd.capivaraos.org/RPM-GPG-KEY-capivaraos-herd
gpg --show-keys --with-colons RPM-GPG-KEY-capivaraos-herd | grep -c '^pub'
gpg --show-keys --with-fingerprint RPM-GPG-KEY-capivaraos-herd
```

O segundo comando (`grep -c`) **tem de mostrar `1`**: o arquivo tem exatamente
uma chave. O terceiro mostra essa chave, e a impressão digital dela **tem de
ser exatamente**:

```
C852 6722 0A81 18C9 864F  0648 BE5B 82AD C8A2 85C0
```

!!! danger "Se a contagem não for 1 ou a impressão digital não bater"
    Pare. Esse arquivo não é a chave do Herd. Não use a imagem e avise pelo
    [canal de segurança](https://www.capivaraos.org/pt/trust-center.html).

**b) Verifique a assinatura só contra essa chave** (todos os arquivos na mesma
pasta):

```bash
gpg --dearmor < RPM-GPG-KEY-capivaraos-herd > herd-key.gpg
LC_ALL=C gpgv --keyring ./herd-key.gpg SHA256SUMS.asc SHA256SUMS
# esperado: Good signature from "CapivaraOS Herd (repo signing key) ..."
```

O `gpgv` confere a assinatura só contra a chave que você conferiu no passo (a),
então um **Good signature** aqui quer dizer que o `SHA256SUMS` foi assinado com a
chave do Herd. O `LC_ALL=C` só deixa a saída em inglês, para comparar com este
texto.

Depois, rode a conferência de checksum do passo 1 com este `SHA256SUMS`.

!!! note "A chave do Herd é separada da chave do desktop"
    As edições desktop são assinadas com outra chave. Use a chave do Herd para
    as imagens e as atualizações do Herd.

## Próximo passo

- Servidor físico/VM → [Instalação via ISO](instalacao-iso.md)
- Nuvem/virtualização → [Implantação em nuvem/VM](instalacao-nuvem.md)

---

*Conteúdo produzido com auxílio de IA, em revisão contínua. Encontrou um erro? [Abra uma issue](https://github.com/capivaraos/capivaraos-herd/issues).*
