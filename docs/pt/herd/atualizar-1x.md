# Atualizar do Herd 1.0.1

O Herd 1.0.1 é baseado no Fedora 44 e não tem repositório do Herd, então não
recebe atualizações do Herd. Este guia leva um Herd 1.0.1 instalado para o
Herd 2.0, no Fedora 45, **sem reinstalar**.

São duas etapas:

1. passar a base do Fedora 44 para o Fedora 45;
2. adicionar o repositório assinado do Herd e instalar os pacotes do Herd 2.0.

Depois disso, o servidor tem os pacotes do Herd 2.0, as atualizações
automáticas estão ligadas e as atualizações do Herd vêm do repositório
assinado. Veja [Atualizar o Herd](atualizar.md).

!!! warning "Faça um backup antes"
    Um upgrade de geração passa todo o sistema base do Fedora 44 para o
    Fedora 45. Faça um backup antes de começar e planeje restaurá-lo se o
    upgrade falhar. Veja
    [Snapshot e recuperação](atualizar.md#snapshot-e-recuperacao).

Rode todos os comandos abaixo no servidor, com o usuário administrador. É
preciso acesso à internet.

## Etapa 1: do Fedora 44 para o Fedora 45

Atualize o Fedora 44 e reinicie:

```bash
sudo dnf upgrade --refresh
sudo systemctl reboot
```

Durante a atualização, o dnf pode pedir para importar a chave de assinatura do
Fedora 44, que ele lê de `/etc/pki/rpm-gpg/RPM-GPG-KEY-fedora-44-x86_64`.
Responda sim.

Baixe o upgrade para o Fedora 45. O servidor continua no ar durante o download:

```bash
sudo dnf system-upgrade download --releasever=45
```

O dnf pede para confirmar o download e depois para importar a chave de
assinatura do Fedora 45, que ele lê de um arquivo que já está no servidor
(`/etc/pki/rpm-gpg/RPM-GPG-KEY-fedora-45-x86_64`). Responda sim às duas.

Aplique. O servidor reinicia para instalar o upgrade, o que leva um tempo, e
volta já no Fedora 45:

```bash
sudo dnf offline reboot
```

O dnf pede para confirmar a reinicialização.

Quando ele voltar, confira a base:

```bash
rpm -E %fedora
```

Tem de mostrar `45`. Neste ponto o `/etc/os-release` ainda diz
`VERSION="1.0.1 (Community)"`; a etapa 2 muda isso.

## Etapa 2: o repositório do Herd e os pacotes do Herd 2.0

### 1. Baixe a chave do Herd e confira a impressão digital

```bash
curl -fsSLO https://repo-herd.capivaraos.org/RPM-GPG-KEY-capivaraos-herd
gpg --show-keys --with-colons RPM-GPG-KEY-capivaraos-herd | grep -c '^pub'
gpg --show-keys --with-fingerprint RPM-GPG-KEY-capivaraos-herd
```

O segundo comando (`grep -c`) **tem de mostrar `1`**: o arquivo tem exatamente
uma chave. O terceiro mostra essa chave, e a impressão digital dela **tem de
ser exatamente**:

```
C852 6722 0A81 18C9 864F  0648 BE5B 82AD C8A2 85C0
```

!!! danger "Se a contagem não for 1 ou a impressão digital não bater"
    Pare aqui e não importe o arquivo. Ele não é a chave do Herd. Reporte pelo
    [canal privado de segurança](https://www.capivaraos.org/pt/trust-center.html).

Se bater, importe:

```bash
sudo rpmkeys --import RPM-GPG-KEY-capivaraos-herd
```

### 2. Baixe o pacote do repositório e confira a assinatura

```bash
curl -fsSLO https://github.com/capivaraos/repo-herd/releases/download/herd-f45-x86_64-stable/capivaraos-herd-repos-2.0.0-2.fc45.herd.noarch.rpm
LC_ALL=C rpm -K capivaraos-herd-repos-2.0.0-2.fc45.herd.noarch.rpm
rpm -qp --qf '%{RSAHEADER:pgpsig}\n' capivaraos-herd-repos-2.0.0-2.fc45.herd.noarch.rpm
```

A primeira conferência tem de terminar em `digests signatures OK`, e a segunda
em `Key ID be5b82adc8a285c0`, a chave do Herd. O `LC_ALL=C` só deixa a saída em
inglês, para comparar com este texto.

!!! warning "Não pule esta conferência"
    O dnf não confere a assinatura de um pacote que você instala a partir de um
    arquivo. Estas duas linhas são a conferência. Só instale o arquivo se as duas
    baterem; senão, pare e reporte.

Instale:

```bash
sudo dnf install ./capivaraos-herd-repos-2.0.0-2.fc45.herd.noarch.rpm
```

Isso adiciona o repositório do Herd e os arquivos das duas chaves públicas do
Herd, a principal e a reserva. O primeiro comando dnf do próximo passo
pergunta, uma vez para cada chave, se deve importá-la. Responda sim só se as
impressões digitais forem `C852 6722 0A81 18C9 864F  0648 BE5B 82AD C8A2 85C0`
e `F4EF 73FA F615 7390 BB65  38FF A765 804F 4ADA 507E` (o dnf mostra sem
espaços); senão, responda não e reporte.

### 3. Atualize os pacotes do Herd

```bash
sudo dnf upgrade --refresh
sudo dnf install capivaraos-herd-autoupdate capivaraos-herd-upgrade
sudo systemctl enable --now dnf5-automatic.timer
```

O primeiro comando leva os pacotes de identidade visual e de fortalecimento para
a 2.0. O segundo adiciona os dois pacotes novos da 2.0: a política de
atualização automática de segurança e o `herd-upgrade`. O terceiro liga as
atualizações automáticas.

## Confira o resultado

```bash
grep -E '^(VERSION|VERSION_ID)=' /etc/os-release
sudo rpm -qa 'capivaraos-herd*' --qf '%{NAME}-%{VERSION}  %{RSAHEADER:pgpsig}\n'
systemctl is-active dnf5-automatic.timer
```

- `VERSION="2.0.0 (Community)"` e `VERSION_ID=45`;
- cinco pacotes `capivaraos-herd-*`, todos na 2.0.0, cada um com `Key ID be5b82adc8a285c0`;
- o timer está `active`.

Agora você pode seguir o resto da documentação da 2.0, começando por
[Atualizar o Herd](atualizar.md).
