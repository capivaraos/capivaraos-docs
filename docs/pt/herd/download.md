# Download e verificação

## Escolha a imagem

O HERD Community 1.0.0 é distribuído em duas imagens para **x86_64**:

| Imagem | Arquivo | Quando usar |
|---|---|---|
| **ISO instaladora** | `CapivaraOS-HERD-1.0.0-x86_64.installer.iso` | Instalar em servidor físico ou VM a partir do zero |
| **Disco qcow2** | `CapivaraOS-HERD-1.0.0-x86_64.qcow2` | Subir rápido em nuvem/virtualização com `cloud-init` |

Baixe em:

- **Site:** [capivaraos.org — Download](https://capivaraos.org/pt/download.html)
- **SourceForge:** [sourceforge.net/projects/capivaraos](https://sourceforge.net/projects/capivaraos/)

## Verifique a integridade

Sempre confira o **SHA-256** do arquivo baixado contra o valor publicado ao lado
da imagem (arquivo `.sha256` / `CHECKSUM`).

=== "Linux / macOS"

    ```bash
    sha256sum CapivaraOS-HERD-1.0.0-x86_64.installer.iso
    # compare a saída com o valor publicado
    ```

=== "Windows (PowerShell)"

    ```powershell
    Get-FileHash .\CapivaraOS-HERD-1.0.0-x86_64.installer.iso -Algorithm SHA256
    ```

Se houver um arquivo `CHECKSUM` com as somas, dá para verificar tudo de uma vez:

```bash
sha256sum -c CHECKSUM
```

!!! warning "Se o checksum não bater"
    Não use a imagem. Um hash diferente indica download corrompido ou arquivo
    adulterado — baixe de novo, de preferência da fonte oficial.

## Próximo passo

- Servidor físico/VM → [Instalação via ISO](instalacao-iso.md)
- Nuvem/virtualização → [Implantação em nuvem/VM](instalacao-nuvem.md)

---

*Conteúdo produzido com auxílio de IA, em revisão contínua. Encontrou um erro? [Abra uma issue](https://github.com/capivaraos/capivaraos-herd/issues).*
