# Verificar o download

Antes de gravar o pendrive, vale conferir o arquivo que você baixou. São duas
perguntas diferentes, e cada uma tem a sua resposta:

- **O arquivo chegou inteiro?** → o **checksum SHA-256**.
- **O arquivo é mesmo nosso?** → a **assinatura GPG**.

A primeira pega download corrompido ou pela metade (a causa mais comum de uma ISO
que não inicia). A segunda prova que a imagem foi publicada pelo CapivaraOS, e não
trocada por outra pessoa. A segunda é opcional, mas é rápida e recomendada.

## O que baixar

Na pasta de cada edição ([capivaraos.org/pt/download.html](https://capivaraos.org/pt/download.html)
ou [SourceForge](https://sourceforge.net/projects/capivaraos/files/)) ficam, ao
lado da ISO:

| Arquivo | Para quê |
|---|---|
| `CapivaraOS-<Edição>-2.0.0-x86_64.iso` | a imagem |
| `...iso.sha256` | o checksum publicado |
| `...iso.sha256.asc` | a assinatura GPG do checksum |
| `...iso.torrent` | download por BitTorrent (confere sozinho enquanto baixa) |

!!! tip "O torrent já verifica para você"
    Se baixar pelo `.torrent`, o próprio cliente BitTorrent confere cada pedaço
    contra o hash enquanto baixa — um download por torrent que termina já está
    íntegro. Ainda assim, a assinatura GPG (abaixo) continua valendo a pena.

## 1. O arquivo chegou inteiro? (checksum)

Baixe o `.sha256` na **mesma pasta** da ISO e rode:

=== "Linux / macOS"

    ```bash
    sha256sum -c CapivaraOS-Snout-2.0.0-x86_64.iso.sha256
    # deve responder: CapivaraOS-Snout-2.0.0-x86_64.iso: OK
    ```

    (no macOS use `shasum -a 256 -c` se não tiver o `sha256sum`.)

=== "Windows (PowerShell)"

    ```powershell
    Get-FileHash .\CapivaraOS-Snout-2.0.0-x86_64.iso -Algorithm SHA256
    # compare a saída com o valor da sua edição na tabela abaixo
    ```

Os valores publicados da **2.0.0** são:

```
Marsh   79666af3bee34b6ad9477c3e4f49aee5e0d621931b80163bb88e30dc2b03dd22
Pup     13acef6ce12cb0120f5cdb8890ef2d3a5f7607e324516611afe4cfb4a481209f
Snout   8e25df502c4a22b6ac898fde9347aa03c47a28220a2bcdd83dc1981e5e212bd1
```

!!! warning "Se o checksum não bater"
    Não use a imagem e não grave o pendrive — um hash diferente é download
    corrompido ou arquivo adulterado. Baixe de novo, de preferência pelo
    **torrent**, que verifica sozinho.

## 2. O arquivo é mesmo nosso? (assinatura GPG)

O checksum prova que o arquivo chegou inteiro, mas não prova **quem** o publicou:
quem trocasse a imagem publicaria o checksum novo junto. Quem responde isso é a
assinatura. O `.sha256.asc` é a assinatura GPG do arquivo de checksum, feita com a
nossa chave — cuja **parte privada nunca saiu de um ambiente offline**.

**a) Baixe e importe a nossa chave pública:**

```bash
curl -O https://repo.capivaraos.org/RPM-GPG-KEY-capivaraos
gpg --import RPM-GPG-KEY-capivaraos
```

**b) Confira a impressão digital.** Este é o passo que importa — tem que ser
**exatamente**:

```
75FA 3340 40FD 0150 4AAD  9337 E07F 78EA 4070 11BD
```

```bash
gpg --show-keys --with-fingerprint RPM-GPG-KEY-capivaraos
# uid: CapivaraOS (repo signing key) ... <capivaraos-bot@users.noreply.github.com>
```

!!! danger "Se a impressão digital não bater"
    Pare. Uma chave diferente significa que o arquivo está assinado por quem não
    somos nós. Não grave a imagem e avise pelo
    [canal de segurança](https://www.capivaraos.org/pt/trust-center.html).

**c) Verifique a assinatura do checksum** (os dois arquivos na mesma pasta):

```bash
gpg --verify CapivaraOS-Snout-2.0.0-x86_64.iso.sha256.asc \
             CapivaraOS-Snout-2.0.0-x86_64.iso.sha256
# esperado: Good signature from "CapivaraOS (repo signing key) ..."
```

O aviso `WARNING: This key is not certified with a trusted signature` é normal:
ele só diz que você não assinou a nossa chave na sua *teia de confiança*. O que
importa é o **Good signature** e a impressão digital conferida no passo (b).

!!! note "A mesma chave assina as atualizações"
    É a mesma chave que assina o [repositório de atualizações](../atualizar/index.md)
    do sistema instalado. Confira a impressão digital uma vez e você reconhece
    tanto a ISO quanto os updates.

## Próximo passo

Arquivo íntegro e assinatura conferida? Siga para o
[**Guia de instalação**](guia.md) para gravar o pendrive e instalar.
