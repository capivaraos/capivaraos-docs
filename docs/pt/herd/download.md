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
1a9603ff2d438f7bcf4e1c7ba236f0f4e25b859fa2f5f733ee745b609021bd2d  CapivaraOS-HERD-1.0.0-x86_64.qcow2
754f21ce6b7ec5d4b107047cea016abe7743872eed6862ecec97cf6448fd2e2a  CapivaraOS-HERD-1.0.0-x86_64.installer.iso
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
