# Como o CapivaraOS é construído

Esta página descreve o processo de desenvolvimento do CapivaraOS: que ferramentas
entram, o que é verificado por pessoas e o que você pode conferir por conta
própria. Ela existe para quem precisa avaliar a confiabilidade do sistema antes
de colocá-lo em uso — não é material de divulgação.

## O que você pode verificar sem depender da nossa palavra

| O que | Onde |
|---|---|
| Código das edições desktop | [capivaraos-marsh](https://github.com/capivaraos/capivaraos-marsh), [capivaraos-pup](https://github.com/capivaraos/capivaraos-pup), [capivaraos-snout](https://github.com/capivaraos/capivaraos-snout) |
| Código do servidor | [capivaraos-herd](https://github.com/capivaraos/capivaraos-herd) |
| Esta documentação | [capivaraos-docs](https://github.com/capivaraos/capivaraos-docs) |
| Histórico de mudanças, com identificador de rastreamento por item | [Changelog](../changelog.md) |
| Assinatura das imagens | arquivos `.sha256` e `.sha256.asc` ao lado de cada ISO — ver [Verificar o download](../instalacao/verificar-download.md) |

Os repositórios são públicos e podem ser clonados anonimamente. Se um texto
nosso e o código discordarem, o código é a fonte.

## O caminho de uma mudança

1. **Branch** a partir da `main`, com nome descritivo.
2. **Pull request**, onde a mudança é lida inteira antes de entrar. O fluxo está
   escrito no `CONTRIBUTING.md` de cada repositório — por exemplo, o
   [do Herd](https://github.com/capivaraos/capivaraos-herd/blob/main/CONTRIBUTING.md) —
   e vale também para quem mantém o projeto.
3. **Verificação na prática** antes de publicar: o que a versão passa a oferecer
   é exercitado em máquina virtual ou instalação real. Recurso que não foi
   provado não é anunciado nem documentado como existente.
4. **Release** é uma *tag* mais a imagem publicada — nunca "o que estiver na
   `main`".

A chave privada que assina os pacotes e o repositório de atualizações não fica
em nenhum repositório nem em nenhum sistema de automação.

## Assistentes de IA no desenvolvimento

O projeto usa assistentes de IA como ferramenta de trabalho. Declaramos isso aqui
porque você tem o direito de saber como o sistema que instala na sua máquina é
feito.

!!! info "Isto não é a IA do produto"
    Esta seção trata de **como o CapivaraOS é desenvolvido**. É assunto
    diferente da **IA local** que o sistema oferece ao usuário — modelos que
    rodam na sua própria máquina, descritos em
    [COS Dev Center](../apps/cos-dev.md). Uma coisa não depende da outra.

**Onde entram:**

- escrita e revisão de código e de empacotamento RPM;
- leitura de configuração de segurança, procurando inconsistência e erro;
- redação e tradução desta documentação;
- apoio na preparação e na análise de testes.

**O que não fazem:**

- não aprovam mudança: quem mescla um pull request é uma pessoa;
- não decidem o que vai para produção;
- não assinam pacotes nem têm acesso à chave privada de assinatura;
- não substituem a verificação em máquina real ou virtual descrita acima.

Em outras palavras: a IA acelera o trabalho dentro de um processo que não aceita
resultado não verificado. Quando uma ferramenta e uma verificação discordam,
vale a verificação.

## No Herd (servidor)

O servidor é usado em ambiente de produção por terceiros, então aqui o critério
é mais estrito.

- **O fortalecimento é código legível, não heurística.** O que o
  `herd-harden` aplica está versionado no repositório — as regras de auditoria,
  as políticas e o script — e pode ser lido linha por linha antes de rodar. Não
  há decisão tomada em tempo de execução por um modelo.
- **A varredura de conformidade usa conteúdo de terceiros.** Os perfis vêm do
  SCAP Security Guide, projeto externo e auditado de forma independente; nós não
  escrevemos os controles que a varredura mede.
- **Análise estática no CI.** O repositório do Herd roda ShellCheck nos scripts
  de shell a cada push e pull request
  ([workflow](https://github.com/capivaraos/capivaraos-herd/blob/main/.github/workflows/shellcheck.yml)).
- **Número publicado tem data, versão e método.** Sem isso, o número não é
  publicado.
- **Vocabulário preciso.** Dizemos *compliance-ready* e nunca *certificado*;
  dizemos *modo FIPS* e nunca *validação FIPS*. A distinção está em
  [Segurança e compliance](../herd/seguranca.md).

## Encontrou um erro?

Erro nesta documentação ou no comportamento do sistema: abra uma issue no
repositório correspondente. Vulnerabilidade de segurança: **não** abra issue
pública — siga o `SECURITY.md` do repositório.

---

*Conteúdo produzido com auxílio de IA, em revisão contínua. Encontrou um erro? [Abra uma issue](https://github.com/capivaraos/capivaraos-docs/issues).*
