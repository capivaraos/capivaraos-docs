# Endurecimento em 1 comando

O Herd já vem [endurecido por padrão](seguranca.md). Quando você precisa ir
**além do baseline** — para atender um padrão específico como CIS ou PCI-DSS —
o `herd-harden` aplica um perfil de conformidade inteiro em **um comando**.

Ele é a contraparte "aplicar" do [`herd-compliance-scan`](seguranca.md#auditar-herd-compliance-scan)
(que só *relata*): usa o mesmo conteúdo do **SCAP Security Guide** (SSG) do
Fedora e aplica a remediação via **Ansible**.

!!! warning "Por padrão, ele NÃO altera nada"
    Sem `--apply`, o `herd-harden` roda em **dry-run** (modo verificação do
    Ansible): mostra o que *mudaria*, sem tocar no sistema. Só aplica de verdade
    com `--apply`, explícito. Sempre revise o dry-run antes.

## Perfis disponíveis

| Perfil | Alvo | Regras (aprox.) | Observação |
|---|---|---|---|
| `standard` | Baseline do Herd (padrão) | ~76 | Já aplicado de fábrica |
| `ospp` | Protection Profile p/ SO de uso geral (NIAP/Common Criteria) | ~208 | Perfil "oficial" (não-rascunho) |
| `cis-l1` | CIS Nível 1 — Servidor | ~324 | Rascunho do SSG para Fedora |
| `cis-l2` | CIS Nível 2 — Servidor | ~438 | Rascunho; mais agressivo |
| `pci` | PCI-DSS v3.2.1 | ~121 | Controles para ambientes de cartão |

Você também pode passar o **id técnico completo** do perfil
(`xccdf_org.ssgproject.content_profile_*`). Veja todos com `herd-harden --list`.

!!! info "O que esperar de cada padrão — sem promessas falsas"
    - **STIG:** não há perfil DISA STIG para Fedora (o SSG só o publica para
      RHEL). Se o seu requisito é DoD-adjacente, o **`ospp`** é o baseline mais
      próximo.
    - **CIS:** os perfis CIS para Fedora são **rascunhos** mantidos pela
      comunidade do SSG (o CIS não publica um *benchmark* oficial de Fedora).
      Úteis e sólidos, mas trate-os como o que são.
    - **PCI-DSS:** o perfil embarcado é da **v3.2.1**.
    - O Herd entrega os **controles e a evidência** — deixa o sistema *pronto
      para conformidade*. Conformidade em si é processo, jurídico e organização;
      nenhum SO se declara "certificado" por você.

## Uso

### 1. Ver o que mudaria (dry-run)

```bash
sudo herd-harden cis-l1
```

Isso gera o playbook de remediação do perfil e roda o Ansible em **modo
verificação** (`--check --diff`): você vê cada mudança proposta, nada é aplicado.

### 2. Aplicar

```bash
sudo herd-harden cis-l1 --apply
```

As mudanças são aplicadas neste servidor. A saída também vai para um log:

```
/var/log/herd-harden-<data>.log
```

### 3. Conferir o resultado

Meça o antes/depois com o scanner, no **mesmo perfil**:

```bash
sudo herd-compliance-scan cis-l1
```

### Outros comandos

```bash
herd-harden --list      # lista os perfis do datastream
herd-harden --help      # ajuda resumida
```

!!! tip "Comece pelo mais leve"
    Uma boa progressão é `standard` → `ospp` → `cis-l1` → `pci` (ou `cis-l2`,
    o mais restritivo). Aplique um por vez, sempre revisando o dry-run e testando
    seus serviços depois — perfis mais altos endurecem bastante a superfície.

## Cuidados

- **Rode o dry-run primeiro.** Perfis altos podem restringir algo de que a sua
  aplicação depende. Revise o `--diff` antes de `--apply`.
- **Teste após aplicar.** Confirme SSH, seus serviços e o Cockpit.
- **`herd-harden` requer `ansible-core`** (já vem no Herd). O playbook é gerado a
  partir do datastream vendorado — funciona offline.
- Ajustes específicos que você não quer que um perfil sobrescreva devem ir em
  *drop-ins* próprios (veja [Ajustar](seguranca.md#ajustar)).

## Créditos e licenças de terceiros

Assim como o `herd-compliance-scan`, o `herd-harden` usa o
**[SCAP Security Guide](https://github.com/ComplianceAsCode/content)** do Fedora
(`ssg-fedora-ds.xml`, BSD-3-Clause, © 2012-2017 Red Hat, Inc.). Detalhes de
licença na página de [Segurança e compliance](seguranca.md#creditos-e-licencas-de-terceiros).
Os nomes de perfil vêm do próprio SSG; "CIS" refere-se aos *benchmarks* do
[Center for Internet Security](https://www.cisecurity.org/) — o Herd apenas
executa os perfis correspondentes, sem reproduzir o texto dos *benchmarks*.

---

*Conteúdo produzido com auxílio de IA, em revisão contínua. Encontrou um erro? [Abra uma issue](https://github.com/capivaraos/capivaraos-herd/issues).*
