# COS Config Center

O **COS Config Center** resolve, numa interface gráfica, nove coisas que no Linux normalmente pedem linha de comando. Ele é exclusivo do CapivaraOS e já vem instalado nas três edições desktop.

Para abrir, procure por **COS Config Center** no menu de aplicativos.

Ele pede a senha de administrador apenas nos ajustes que mexem no sistema, e sempre avisa antes.

## Software e drivers

### Codecs e repositórios extras

Liga os repositórios **RPM Fusion** e o **Flathub**, e instala os codecs que o Fedora não pode distribuir de fábrica por questões de patente. É o que faz vídeo e música "simplesmente tocarem".

!!! note "São repositórios de terceiros"
    O RPM Fusion e o Flathub são mantidos pelas comunidades deles, não pelo CapivaraOS nem pelo Fedora. Alguns formatos são cobertos por patente em certos países — vale conferir o que se aplica onde você mora.

## Discos e manutenção

### Liberar espaço em disco

Mostra o que dá para apagar com segurança, separado em duas listas: o que está no **seu usuário** (lixeira, miniaturas, runtimes de Flatpak sem uso, imagens de contêiner) e o que está no **sistema** e pede senha (pacotes já instalados, logs antigos, despejos de falha).

Cada item mostra quanto ocupa antes de você decidir. Nada é removido sem você marcar.

### Discos na inicialização

Monta automaticamente os seus discos e partições extras quando o computador liga, sem você editar o `/etc/fstab` à mão.

### Tamanho dos logs

Limita o quanto os logs do sistema podem ocupar, para eles não crescerem sem parar e encherem o disco.

### Verificação do sistema

Uma checagem rápida de saúde: espaço nos discos, serviços rodando, driver de vídeo em uso e erros desde que a máquina ligou.

No fim ele monta um **relatório para colar no fórum** quando você for pedir ajuda. O relatório **não inclui** o seu nome, o nome do computador nem endereços de rede.

## Teclado e idioma

### Teclado brasileiro

Escolhe entre **ABNT2**, **US Internacional** e **US** — e, no US Internacional, liga a opção de digitar **ç** com `'` seguido de `c`, que é o detalhe que mais incomoda quem escreve em português num teclado americano.

Dá para aplicar o mesmo leiaute também na tela de login.

### Idioma e formatos

Separa duas coisas que costumam vir grudadas: o **idioma** dos menus e dos aplicativos, e os **formatos** de data, número e moeda. Você pode usar o sistema em inglês mantendo data e moeda no padrão brasileiro.

Vale para o sistema inteiro, inclusive a tela de login. Tem efeito depois de sair e entrar de novo.

## Rede e segurança

### Firewall

Mostra o que está aberto para a rede em linguagem direta — "compartilhar pastas na rede", "acesso remoto ao terminal", "servidor web" — em vez de números de porta.

Tem também o **modo estrito**, que fecha as portas acima de 1024 e deixa passar só o que você ligou.

### Acesso remoto e rede

Muda o **nome do computador** na rede, liga ou desliga o **acesso remoto por SSH**, cria a sua **chave SSH** e configura **DNS criptografado**.

!!! warning "Duas ressalvas desta versão"
    O login por senha no SSH continua ligado — nós não alteramos o `sshd_config`. E o DNS criptografado pode impedir a resolução de nomes que só existem na sua rede local; se algo parar de ser encontrado depois de ligar, volte para "Automático".

## Esta versão

O COS Config Center sai com a Série 2 como **0.11.x**, uma primeira versão pública com estes nove módulos. A 1.0.0 virá depois do retorno de quem usar.

**Não entram nesta versão:** driver NVIDIA (isso é com o [COS Dev Center](cos-dev.md)), configuração de dual boot e aceleração de vídeo.

As **atualizações do sistema** também não passam por aqui — elas ficam com a loja de aplicativos ou o `dnf`. Veja [Atualizar o sistema](../atualizar/index.md).
