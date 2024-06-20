# [Grafeno Pagamentos](https://pagamentos.grafeno.digital/)

[![Rails Style Guide](https://img.shields.io/badge/code_style-rubocop-brightgreen.svg)](https://github.com/rubocop/rubocop-rails)
[![Rails Style Guide](https://img.shields.io/badge/code_style-community-brightgreen.svg)](https://rails.rubystyle.guide)
[![Ruby Style Guide](https://img.shields.io/badge/code_style-rubocop-brightgreen.svg)](https://github.com/rubocop/rubocop)
[![Ruby Style Guide](https://img.shields.io/badge/code_style-community-brightgreen.svg)](https://rubystyle.guide)
[![Bugs](https://sonarcloud.io/api/project_badges/measure?project=grafeno-sa_grafeno-pagamentos&amp;metric=bugs&amp;token=9d021402e56bbadba697e2b8e3ce0d2119ef0ab5)](https://sonarcloud.io/summary/new_code?id=grafeno-sa_grafeno-pagamentos)
[![Code Smells](https://sonarcloud.io/api/project_badges/measure?project=grafeno-sa_grafeno-pagamentos&amp;metric=code_smells&amp;token=9d021402e56bbadba697e2b8e3ce0d2119ef0ab5)](https://sonarcloud.io/summary/new_code?id=grafeno-sa_grafeno-pagamentos)
[![Vulnerabilities](https://sonarcloud.io/api/project_badges/measure?project=grafeno-sa_grafeno-pagamentos&amp;metric=vulnerabilities&amp;token=9d021402e56bbadba697e2b8e3ce0d2119ef0ab5)](https://sonarcloud.io/summary/new_code?id=grafeno-sa_grafeno-pagamentos)
[![Coverage](https://sonarcloud.io/api/project_badges/measure?project=grafeno-sa_grafeno-pagamentos&amp;metric=coverage&amp;token=9d021402e56bbadba697e2b8e3ce0d2119ef0ab5)](https://sonarcloud.io/summary/new_code?id=grafeno-sa_grafeno-pagamentos)
[![CI](https://github.com/grafeno-sa/grafeno-pagamentos/actions/workflows/ci.yml/badge.svg)](https://github.com/grafeno-sa/grafeno-pagamentos/actions/workflows/ci.yml)
[![Linter and Labels](https://github.com/grafeno-sa/grafeno-pagamentos/actions/workflows/pr.yml/badge.svg)](https://github.com/grafeno-sa/grafeno-pagamentos/actions/workflows/pr.yml)
[![Check Assets](https://github.com/grafeno-sa/grafeno-pagamentos/actions/workflows/build-cache-to-s3.yml/badge.svg)](https://github.com/grafeno-sa/grafeno-pagamentos/actions/workflows/build-cache-to-s3.yml)

## Índice

1. [Instalando Ruby](#instalando-ruby)
2. [Instalando Bundler](#instalando-bundler)
3. [Instalando Node.js](#instalando-nodejs)
4. [Instalando Yarn](#instalando-yarn)
5. [Instalando o Postgres](#instalando-o-postgres)
6. [Instalando o Redis](#instalando-o-redis)
7. [Clonando o Projeto](#clonando-o-projeto)
8. [Configurando Chaves e Configurações](#configurando-chaves-e-configurações)
9. [Configurando Uso do Banco](#configurando-uso-do-banco)
    - [Sem Docker](#sem-docker)
    - [Com Docker](#com-docker)
10. [Preparando o Projeto](#preparando-o-projeto)
11. [Levantando o Servidor](#levantando-o-servidor)
12. [Troubleshooting](#troubleshooting)
    - [Debian-like](#debian-like)
    - [WSL/Ubuntu](#wslubuntu)
    - [Linux Mint e ElementaryOS](#linux-mint-e-elementaryos)
    - [Solus](#solus)
    - [Arch Linux](#arch-linux)
    - [macOS Catalina 10.15](#macos-catalina-1015)
    - [macOS CHIP M1](#macos-chip-m1)
13. [Docker para Mac M1](#docker-para-mac-m1)

## Instalando Ruby

Utilize um version manager como `rbenv` ou `rvm`.

Versão: 3.1.4

### rvm

[rvm](https://rvm.io/)sh
rvm install 3.1.4

### rbenv

[rbenv](https://github.com/rbenv/rbenv)sh
rbenv install 3.1.4

Se houver problemas no build:sh
git -C ~/.rbenv/plugins/ruby-build pull
RUBY_CONFIGURE_OPTS="--with-jemalloc --enable-yjit --disable-install-doc" rbenv install 3.1.4

## Instalando Bundler

Versão: 2.4.10sh
gem install bundler -v 2.4.10

## Instalando Node.js

Recomendado uso de version manager.

Versão: 16

[nvm](https://github.com/nvm-sh/nvm)sh
nvm install v16

## Instalando Yarn

Instale o Yarn com `npm`, que vem junto com Node.js.sh
npm install --global yarn
Para outras formas de instalação, veja: [Instalação do Yarn](https://classic.yarnpkg.com/lang/en/docs/install/#windows-stable)

## Instalando o Postgres

**_SE FOR UTILIZAR DOCKER, PULE ESTA ETAPA_**

Versão: >= 16.2

[PostgreSQL](https://www.postgresql.org/download/)

## Instalando o Redis

**_SE FOR UTILIZAR DOCKER, PULE ESTA ETAPA_**

Versão: >= 4.0.9

[Redis](https://redis.io/docs/install/install-redis/)

## Clonando o Projeto

Autentique-se através do GitHub CLI.

* [Instalação no Linux](https://github.com/cli/cli/blob/trunk/docs/install_linux.md)
* [Instalação no Mac](https://github.com/cli/cli#installation)

Verifique a instalação com `gh --version` e rode:sh
gh auth login

Após autenticar, clone o projeto:sh
git clone git@github.com:grafeno-sa/grafeno-pagamentos.git

## Configurando Chaves e Configuraçõessh
cd grafeno-pagamentos
cp .env.development .env
cp config/database.yml.example config/database.yml
cp config/internal.pem.example config/internal.pem
cp config/cadastros_public_key.pem.example config/cadastros_public_key.pem
cp config/webhook_private_key.pem.example config/webhook_private_key.pem

## Configurando Uso do Banco

Os serviços do Postgres e Redis estão dockerizados. Utilize conforme necessário.

### Sem Docker

Crie `.env.development.local`:sh
REDIS_URL=redis://localhost:6379

Adicione `.env.development.local` a `.git/info/exclude`.

Configure a senha do Postgres:sh
sudo -u postgres psql postgres
\password
# Insira 'postgres' como senha
\q

### Com Docker

#### Arch Linuxsh
sudo pacman -S docker docker-compose

#### Ubuntu e derivadossh
sudo apt install docker.io curl
sudo curl -L "https://github.com/docker/compose/releases/download/1.24.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose

#### Solussh
sudo eopkg install docker docker-compose

Desabilite Redis e Postgres locais:sh
sudo systemctl disable redis
sudo systemctl stop redis
sudo systemctl disable postgresql
sudo systemctl stop postgresql

Levante os serviços:sh
docker-compose up -d

## Preparando o Projeto

Instale as gemas do projeto:sh
bundle install

Se houver problemas com as gemas `pg` e `capybara`, veja o troubleshooting.

Certifique-se de que seu Node.js é a versão 16:sh
yarn install
rails assets:precompile

Prepare o banco de dados:sh
rails db:create
rails db:migrate

Rodar a seed principal:sh
rails db:seed

Para seeds secundárias/opcionais:sh
rails gdt:populate_development_environment
rails customized_seeds

#### Problemas possíveis:

- Erros de `NilClass` ou número de conta: refaça o banco e rode a seed novamente.
- Erros de `MiniMagick`: veja troubleshooting.

Refaça o banco e rode a seed:sh
rails db:drop db:create db:migrate &amp;&amp; rails db:seed gdt:populate_development_environment

Para rodar testes e linters:sh
bundle exec rubocop
bundle exec erblint app/
yarn eslint
bundle exec rspec

## Levantando o Servidor

Levante o servidor:sh
bin/rails server

Acesse em [http://127.0.0.1:3000](http://127.0.0.1:3000)

## Troubleshooting

### Debian-like

Problemas ao instalar `pg`:sh
sudo apt-get install -y libpq-dev postgresql-client

Problemas ao instalar `capybara-webkit`:sh
sudo apt-get install -y qt5-default libqt5webkit5-dev

### WSL/Ubuntu

Problemas com ImageMagick:sh
magick identify -version

Se não aparecer a versão:sh
sudo apt install imagemagick

Se precisar remover:sh
sudo apt remove imagemagick
sudo apt autoremove

Se estiver tudo ok:sh
which convert
which identify
which magick

Reinstale ImageMagick:sh
sudo apt install imagemagick

### Linux Mint e ElementaryOS

Rode `rvm get head` antes de `rvm install`.

### Solus

Instale o `system.devel` para compilar o Ruby:sh
sudo eopkg install -c system.devel

Instale Ruby 3.1.4 com `rvm install 3.1.4`.

### Arch Linux

Instale pacotes necessários:sh
sudo pacman -S qt5-webkit libpqxx

Se houver erro ao executar `rails db:setup`:sh
Adicione `127.0.0.1 postgres` ao arquivo `/etc/hosts`.

Para problemas com testes de feature:sh
sudo ln -s /usr/bin/google-chrome-stable /usr/bin/google-chrome

Faça downgrade de pacotes:sh
downgrader ghostscript

Escolha a versão 9.26-2.sh
downgrader jbig2dec

Escolha a versão 0.16-1. Adicione ao `/etc/pacman.conf`:Ignorepkg = ghostscript
Ignorepkg = jbig2dec

### macOS Catalina 10.15

Instale Xcode e MacPort, depois instale a versão correta do Qt:sh
sudo port install qt5 qt5-webkit
sudo ln -s /opt/local/libexec/qt5/bin/qmake /usr/local/bin/qmake

Rode `bundle install` ou:sh
QMAKE=/opt/local/libexec/qt5/bin/qmake gem install capybara-webkit -v '1.15.1'

### macOS CHIP M1

Adicione ao `.zshrc`:sh
export LDFLAGS="-L/opt/homebrew/opt/openssl@3/lib"
export CPPFLAGS="-I/opt/homebrew/opt/openssl@3/include"
export PKG_CONFIG_PATH="/opt/homebrew/opt/openssl@3/lib/pkgconfig"
export LDFLAGS="-L/opt/homebrew/opt/zlib/lib:$LDFLAGS"
export CPPFLAGS="-I/opt/homebrew/opt/zlib/include:$CPPFLAGS"
export PKG_CONFIG_PATH="/opt/homebrew/opt/zlib/lib/pkgconfig:$PKG_CONFIG_PATH"

Reinicie o shell:sh
exec $SHELL

Instale dependências:sh
brew install openssl libffi zlib rbenv readline ruby-build

Reinstale se necessário, e reinicie a máquina. Instale o Ruby:sh
rbenv install 3.1.4

Problemas com a gem `ffi`:sh
gem install ffi -v '1.12.2' -- --with-cflags="-Wno-error=implicit-function-declaration"

Problemas com `capybara` e qt:sh
sudo port selfupdate
sudo xcodebuild -license agree
sudo port install qt5 qt5-qtwebkit
sudo ln -s /opt/local/libexec/qt5/bin/qmake /opt/local/bin/qmake

Problemas com `pg`:sh
brew install libpq
echo 'export PATH="/opt/homebrew/opt/libpq/bin:$PATH"' >> ~/.zshrc
brew link libpq --force

Instale gems:sh
DISABLE_SSL=1 bundle install

Instale dependências:sh
brew install imagemagick
brew install vips

Setup do banco:sh
./bin/rails db:create &amp;&amp; ./bin/rails db:migrate
./bin/rails db:seed

Levante o servidor:sh
./bin/rails s

## Docker para Mac M1

Para mais informações, veja a [documentação do Docker para Mac M1](https://docs.docker.com/docker-for-mac/apple-m1/).
