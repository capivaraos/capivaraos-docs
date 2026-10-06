# Changelog

Principais marcos do desenvolvimento das distros do CapivaraOS.

> Este changelog cobre a identidade visual e os ajustes de sistema do CapivaraOS — não inclui o histórico de pacotes upstream do Fedora, KDE, Xfce ou GNOME.

---

## CapivaraOS Marsh

### 1.2.4
*Agosto de 2026*

- Correção: o CapivaraOS Marsh agora instala em computadores com firmware UEFI. Antes, a instalação percorria quase tudo e falhava no passo do carregador de inicialização, porque o instalador não reconhecia o CapivaraOS como um perfil próprio e procurava os arquivos de boot no lugar errado. `(BUG-38)`
- Correção: o sistema instalado passa a iniciar em uma variedade maior de hardware (por exemplo, netbooks com armazenamento eMMC). Antes, em máquinas cujo armazenamento fosse diferente do usado para gerar a imagem, o sistema podia não encontrar o disco no primeiro boot e cair em modo de emergência. `(BUG-40)`

### 1.2.3
*Agosto de 2026*

- Correção de detalhes de licença dos papéis de parede fotográficos.

### 1.2.2
*Julho de 2026*

- Nova identidade visual: logo da capivara caminhando em todo o branding — wallpapers, ícones, tela de boot, tela de login e instalador. `(DOC-6)`
- Ícone do lançador de aplicativos no dock passa a ser a cabeça da capivara, no lugar da logo do KDE. `(DOC-6)`
- Correção: a logo deixa de se sobrepor ao texto na tela de boas-vindas da sessão live.
- Correção: a ISO passa a incluir as atualizações do Fedora, evitando cerca de 1.000 pacotes de atualização no primeiro boot.
- Correção: a logo e o crédito de autoria dos papéis de parede fotográficos deixam de ser cortados em telas 4:3.
- Correção: a arte da logo deixa de aparecer como opção no seletor de papel de parede.

### 1.1.2
*Junho de 2026*

- Plymouth funcional — animação de boot com a logo do CapivaraOS exibe corretamente (corrigido: pacote `plymouth-plugin-script` ausente).
- Título correto no GRUB — o menu de inicialização mostra "CapivaraOS Marsh" permanentemente, mesmo após atualizações do sistema.
- Tela de atualização offline — Plymouth exibe percentual de progresso durante atualizações aplicadas no boot.
- Correção de duplicidade passageira no dock inferior logo após o login.
- Tela de boas-vindas do Plasma não abre mais no primeiro login.

### 1.1.0
*Junho de 2026*

- Primeira versão portada para Fedora 44, a partir da base original em Debian/trixie.

---

## CapivaraOS Pup

### 1.1.8
*Agosto de 2026*

- Correção: o CapivaraOS Pup agora instala em computadores com firmware UEFI. Antes, a instalação percorria quase tudo e falhava no passo do carregador de inicialização, porque o instalador não reconhecia o CapivaraOS como um perfil próprio e procurava os arquivos de boot no lugar errado. `(BUG-38)`
- Correção: o sistema instalado passa a iniciar em uma variedade maior de hardware (por exemplo, netbooks com armazenamento eMMC). Antes, em máquinas cujo armazenamento fosse diferente do usado para gerar a imagem, o sistema podia não encontrar o disco no primeiro boot e cair em modo de emergência. `(BUG-40)`

### 1.1.7
*Agosto de 2026*

- Correção: o papel de parede do CapivaraOS passa a ser aplicado automaticamente já no primeiro login (live e instalado). Antes, era preciso trocá-lo manualmente uma vez para que ele ficasse.

### 1.1.2
*Julho de 2026*

- Primeira versão beta pública do Pup, disponível para download.
- Nova identidade visual: logo da capivara caminhando em todo o branding — wallpapers, ícones, tela de boot, tela de login e instalador. `(DOC-6)`
- Correção: a ISO passa a incluir as atualizações do Fedora. O repositório de updates era ignorado em silêncio por usar um nome reservado pelo Anaconda, e a imagem saía apenas com o Fedora 44 original.
- Correção: a logo e o crédito de autoria dos papéis de parede fotográficos deixam de ser cortados em telas 4:3.
- Correção: a identificação do sistema no `os-release` e no GRUB agora resiste a atualizações futuras do Fedora.

### 1.0.0
*Junho de 2026*

- Primeira versão da spin Xfce (Fedora 44), voltada para computadores com pelo menos 4 GB de RAM.
- Wallpapers exclusivos: 13 planos de fundo de cor sólida com a logo CapivaraOS e fotos de capivaras do Wikimedia Commons (CC BY/CC BY-SA), com créditos embutidos nas imagens.
- Branding completo no instalador gráfico — exibe logo e cores do CapivaraOS em vez do Fedora.
- Tema Plymouth próprio no boot e shutdown.
- Tela de login LightDM com identidade visual CapivaraOS.
- Sistema identificado corretamente como CapivaraOS Pup no `os-release` e no GRUB.

---

## CapivaraOS Snout

### 1.1.15
*Agosto de 2026*

- Correção: a tela de login volta a aparecer normalmente. Antes, quando o início automático de sessão estava desativado, o sistema não mostrava a tela de login e entrava numa sessão vazia — sem a lista de usuários e sem campo de senha —, e nesse estado não era possível abrir o Terminal. Causa: uma linha faltava na configuração da tela de login do GNOME, introduzida ao personalizar o papel de parede do login. `(BUG-42)`
- O Terminal passa a vir fixado na barra de aplicativos (dock), facilitando o acesso.

### 1.1.9
*Agosto de 2026*

- Correção: o CapivaraOS Snout agora instala em computadores com firmware UEFI. Antes, a instalação percorria quase tudo e falhava no passo do carregador de inicialização, porque o instalador não reconhecia o CapivaraOS como um perfil próprio e procurava os arquivos de boot no lugar errado. `(BUG-38)`
- Correção: o sistema instalado passa a iniciar em uma variedade maior de hardware (por exemplo, netbooks com armazenamento eMMC). Antes, em máquinas cujo armazenamento fosse diferente do usado para gerar a imagem, o sistema podia não encontrar o disco no primeiro boot e cair em modo de emergência. `(BUG-40)`

### 1.1.6
*Agosto de 2026*

- Correção de detalhes de licença dos papéis de parede fotográficos.

### 1.1.5
*Julho de 2026*

- Primeira versão beta pública do Snout, disponível para download.
- Nova identidade visual: logo da capivara caminhando em todo o branding — wallpapers, ícones, tela de boot, tela de login GDM e instalador. `(DOC-6)`
- Correção: a ISO passa a incluir as atualizações do Fedora. O repositório de updates era ignorado em silêncio por usar um nome reservado pelo Anaconda.
- Correção: a logo do painel "Sobre" do GNOME deixa de sair com as bordas cortadas.
- Correção: a identificação do sistema e a logo do CapivaraOS agora resistem a atualizações futuras do Fedora — três mecanismos que deveriam reaplicá-las nunca chegavam a rodar.

### 1.0.0
*Junho de 2026*

- Primeira versão da spin GNOME, com pacote de identidade visual (`capivaraos-branding`), wallpapers, tema de boot/login próprios e fundo do GDM via dconf.
- Ambiente GNOME padrão do Fedora Workstation, sem dock, menu global ou temas de terceiros.

---

## Herd by CapivaraOS

### 1.0.1
*Agosto de 2026*

- Fortalecimento em 1 comando: novo `herd-harden`, que aplica perfis do SCAP Security Guide (OSPP, CIS, PCI-DSS) via Ansible, com pré-visualização (dry-run) por padrão. `(FEAT-97)`
- Relatório de conformidade com evidência: o `herd-compliance-scan` passa a aceitar apelidos de perfil e a gerar também um arquivo ARF (evidência para auditoria), além do HTML/XML. `(FEAT-97)`
- Modo FIPS documentado para o Fedora 44 (via `fips=1` no kernel) e criptografia de disco (LUKS) como opções opt-in.
- A marca da linha passa a ser **Herd by CapivaraOS**.

### 1.0.0
*Agosto de 2026*

- Primeira versão estável da linha de servidor: base Fedora 44 headless, SELinux enforcing, SSH fortalecido, firewall restritivo, auditoria ativa e console web Cockpit incluído. `(PROD-4)`
- Imagens qcow2 (nuvem/cloud-init) e ISO instaladora brandeada (x86_64). `(PROD-4)`
