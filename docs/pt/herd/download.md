# Download e verificação

## Escolha a imagem

O Herd Community 1.0.1 é distribuído em duas imagens para **x86_64**:

| Imagem | Arquivo | Quando usar |
|---|---|---|
| **ISO instaladora** | `CapivaraOS-HERD-1.0.1-x86_64.installer.iso` | Instalar em servidor físico ou VM a partir do zero |
| **Disco qcow2** | `CapivaraOS-HERD-1.0.1-x86_64.qcow2` | Subir rápido em nuvem/virtualização com `cloud-init` |

Baixe em:

- **Site:** [capivaraos.org — Download](https://capivaraos.org/pt/download.html)
- **SourceForge:** [pasta CapivaraOS-HERD/1.0.1](https://sourceforge.net/projects/capivaraos/files/CapivaraOS-HERD/1.0.1/)

## Verifique a integridade

Sempre confira o **SHA-256** do arquivo baixado contra o valor publicado. As
somas das duas imagens estão no arquivo `SHA256SUMS`, na mesma pasta do
download:

```
395d06472e47b9b572769fe35f295806da01abb6536a89e5b5b82a0ec757776b  CapivaraOS-HERD-1.0.1-x86_64.qcow2
8b5283c9ff2115d30a7983b9641ac15fbfa66b90c57c35ef02d8b0517615d172  CapivaraOS-HERD-1.0.1-x86_64.installer.iso
```

=== "Linux / macOS"

    ```bash
    # baixe também o SHA256SUMS na mesma pasta e rode:
    sha256sum -c SHA256SUMS
    # deve responder "OK" para o(s) arquivo(s) que você baixou
    ```

=== "Windows (PowerShell)"

    ```powershell
    Get-FileHash .\CapivaraOS-HERD-1.0.1-x86_64.installer.iso -Algorithm SHA256
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
