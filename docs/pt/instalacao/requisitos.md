# Requisitos de hardware

Valores para a **Série 2** (Fedora 45). As três edições são **x86_64**.

## Marsh (KDE Plasma)

| Componente | Mínimo | Recomendado |
|---|---|---|
| CPU | 64-bit dual-core | 64-bit quad-core |
| RAM | 2 GB | 4 GB ou mais |
| Armazenamento | 20 GB | 40 GB ou mais |
| Vídeo | Suporte a aceleração 3D | GPU dedicada |

## Pup (Xfce)

| Componente | Mínimo | Recomendado |
|---|---|---|
| CPU | 64-bit single-core | 64-bit dual-core |
| RAM | 1 GB | 2 GB ou mais |
| Armazenamento | 15 GB | 30 GB ou mais |

## Snout (GNOME)

| Componente | Mínimo | Recomendado |
|---|---|---|
| CPU | 64-bit dual-core | 64-bit quad-core |
| RAM | 2 GB | 4 GB ou mais |
| Armazenamento | 20 GB | 40 GB ou mais |
| Vídeo | Suporte a aceleração 3D | GPU dedicada |

## Compatibilidade

O CapivaraOS é baseado no Fedora e herda sua compatibilidade de hardware. Processadores Intel, AMD e suporte experimental a ARM.

!!! note "Modo Live"
    Você pode testar o CapivaraOS sem instalar, iniciando pelo pendrive. Nesse caso, recomendamos pelo menos 2 GB de RAM.

!!! warning "Se for usar a IA local"
    O armazenamento das tabelas acima cobre o sistema e o uso comum. A **IA local** do [COS Dev Center](../apps/cos-dev.md) baixa vários gigabytes a mais — some pelo menos **20 GB** ao que você reservou, e conte com mais conforme os modelos que instalar.

!!! tip "Por que Btrfs"
    O instalador usa **Btrfs** por padrão. É o que permite o instantâneo automático antes de cada atualização de geração, e é a razão de o espaço recomendado ser mais folgado que o mínimo.
