# Suporte e ciclo de vida

## Edição

O **Herd Community** é **gratuito e de código aberto** (o código sob GPLv3; nome
e logotipo são marca do projeto). É mantido pela comunidade, em regime de
**melhor esforço** — sem SLA contratual.

## Ciclo de vida

- O Herd **1.x** é baseado no **Fedora 44** e acompanha as atualizações de
  segurança dessa base. Mantenha o sistema atualizado com `dnf upgrade`.
- A **geração 2** (Herd 2.x) migrará para uma base Fedora mais nova, lançada
  quando estiver madura.

!!! tip "Mantenha atualizado"
    A melhor postura de segurança é um sistema em dia. Rode `dnf upgrade`
    regularmente (ou use o módulo **Atualizações** do [Cockpit](cockpit.md)).

## Reportar bugs

Encontrou um problema? Abra uma issue no GitHub:

- **[github.com/capivaraos/capivaraos-herd/issues](https://github.com/capivaraos/capivaraos-herd/issues)**

Ajuda muito incluir:

- versão (`cat /etc/os-release`);
- como reproduzir;
- logs relevantes (`journalctl -b -p err`, ou do serviço afetado);
- se aplicável, a saída do `herd-compliance-scan`.

## Comunidade

- **Site:** [capivaraos.org](https://capivaraos.org)
- **Notícias:** [capivaraos.org/pt/blog.html](https://capivaraos.org/pt/blog.html)
- **Redes:** Instagram, X e YouTube (`@CapivaraOS` / `@Capivara-OS`)

## Contribua

O CapivaraOS é um projeto brasileiro aberto. Relatos de bug, melhorias na
documentação, testes e divulgação são todos bem-vindos — veja a página
[Contribua](../contribua.md).

## Perguntas frequentes

??? question "Consigo entrar por SSH com senha logo após instalar?"
    Não — o SSH vem **só por chave** (`PasswordAuthentication no`). Adicione sua
    chave pelo console/Cockpit, ou (com ressalvas) habilite senha conforme
    [Segurança › SSH](seguranca.md#ssh).

??? question "O Cockpit aceita login de root?"
    Não. Use o usuário administrador (grupo `wheel`) e clique em **Ativar acesso
    administrativo**.

??? question "Tem versão para ARM (aarch64)?"
    Ainda não no 1.0.0 — o suporte a aarch64 chega na **1.1**.

??? question "Posso usar em produção comercial?"
    Sim. O software é livre (GPLv3) e pode ser usado para qualquer finalidade,
    inclusive comercial. Só a **marca** (nome/logo) é protegida — veja o
    `TRADEMARK.md` do repositório.

??? question "Como faço um servidor web/banco/container?"
    O Herd é uma base de servidor enxuta. Instale o que precisar com `dnf`
    (`nginx`, `podman`, `postgresql-server`, etc.) e libere as portas no
    [firewall](administracao.md#firewall).

---

*Conteúdo produzido com auxílio de IA, em revisão contínua. Encontrou um erro? [Abra uma issue](https://github.com/capivaraos/capivaraos-herd/issues).*
