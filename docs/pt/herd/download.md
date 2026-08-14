# Download e verificação

## Escolha a imagem

O HERD Community 1.0.0 é distribuído em duas imagens para **x86_64**:

| Imagem | Arquivo | Quando usar |
|---|---|---|
| **ISO instaladora** | `CapivaraOS-HERD-1.0.0-x86_64.installer.iso` | Instalar em servidor físico ou VM a partir do zero |
| **Disco qcow2** | `CapivaraOS-HERD-1.0.0-x86_64.qcow2` | Subir rápido em nuvem/virtualização com `cloud-init` |

Baixe em:

- **Site:** [capivaraos.org — Download](https://capivaraos.org/pt/download.html)
- **SourceForge:** [pasta CapivaraOS-HERD/1.0.0](https://sourceforge.net/projects/capivaraos/files/CapivaraOS-HERD/1.0.0/)

## Verifique a integridade

Sempre confira o **SHA-256** do arquivo baixado contra o valor publicado. As
somas das duas imagens estão no arquivo `SHA256SUMS`, na mesma pasta do
download:

```
b97a272ee6f0e91fa609e49d1184e4a4149f61667588a4c13f4ce87c03339123  CapivaraOS-HERD-1.0.0-x86_64.qcow2
9a4c1b9957678e8aa25c1187eb962bcb8460c9415f8b0007de396536deeaa2c3  CapivaraOS-HERD-1.0.0-x86_64.installer.iso
```

=== "Linux / macOS"

    ```bash
    # baixe também o SHA256SUMS na mesma pasta e rode:
    sha256sum -c SHA256SUMS
    # deve responder "OK" para o(s) arquivo(s) que você baixou
    ```

=== "Windows (PowerShell)"

    ```powershell
    Get-FileHash .\CapivaraOS-HERD-1.0.0-x86_64.installer.iso -Algorithm SHA256
    # compare a saída com o valor acima
    ```

!!! warning "Se o checksum não bater"
    Não use a imagem. Um hash diferente indica download corrompido ou arquivo
    adulterado — baixe de novo, de preferência da fonte oficial.

## Próximo passo

- Servidor físico/VM → [Instalação via ISO](instalacao-iso.md)
- Nuvem/virtualização → [Implantação em nuvem/VM](instalacao-nuvem.md)

---

*Conteúdo produzido com auxílio de IA, em revisão contínua. Encontrou um erro? [Abra uma issue](https://github.com/capivaraos/capivaraos-herd/issues).*
