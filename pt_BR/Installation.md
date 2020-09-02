# Instalação

Aqui você aprenderá como instalar o Wpanel de diferentes maneiras, sendo as principais a instalação em um ambiente de desenvolvimento e em um ambiente de produção.

# Sumário

1. [Início rápido (PHP + SQLite)](#quick-start)
2. [Criando um ambiente de desenvolvimento (Linux only)](#ambiente-de-desenvolvimento)
    1. [Clone o repositório](#clone-o-repositorio)
    2. [Instale as dependências](#instale-as-dependencias)
    3. [Servidor de desenvolvimento](#servidor-de-desenvolvimento)
    4. [Banco de dados](#banco-de-dados)
    5. [Execute o Setup do Wpanel](#execute-o-setup-do-wpanel)
3. [Publicando em produçao](#publicando-em-produção)
    1. [Enviando os arquivos para o servidor](#enviando-os-arquivos-para-o-servidor)
    2. [Banco de dados de produção](#banco-de-dados-de-produção)
    3. [Mude o Wpanel para o modo de produção](#mude-o-wpanel-para-o-modo-de-produção)
    4. [Execute o Setup do Wpanel em produção](#execute-o-setup-do-wpanel-em-produção)

# Quick Start
O jeito mais fácil de ter o Wpanel CMS pronto para começar a desenvolver seu projeto é através do [Composer](https://getcomposer.org), com o banco de dados SQLite, utilizando o servidor embutido do PHP. Para isto siga os passos abaixo.

- Execute o composer para criar seu projeto:
```sh
composer create-project "wpanel/wpanel4-cms" Blog
```

- Acesse a pasta Blog, criada no passo anterior e execute o script de start: 
```sh
cd Blog
composer run dev
```
- Execute a instalação do Wpanel com acessando http://localhost:8080/index.php/setup
- Preencha o formulário com os dados para o administrador (ROOT)
- Acesse o admin utilizando os dados informados no passo anterior
- Pronto, o Wpanel já está funcionando em http://localhost:8080

# Ambiente de desenvolvimento

Siga os passos abaixo para montar seu ambiente de desenvolvimento. Você poderá usar o servidor embutido do PHP ou usar um servidor nativo. Também é possível usar [Docker](https://www.docker.com/).

Por padrão o Wpanel já vem com o banco de dados SQLite configurado, isto pode ser alterado sem nenhuma dificuldade. Os passos específicos para configuração do banco de dados MySQL serãoa descritos mais abaixo.

## Clone o repositório

Execute o comando abaixo para clonar o repositorio do Wpanel:

```sh
git clone https://github.com/wpanel/wpanel4-cms.git seuprojeto
```

Alternativamente você pode fazer o download do projeto [neste link](https://github.com/wpanel/wpanel4-cms/archive/master.zip).


## Instale as dependências

O Wpanel utiliza o gerenciador de dependências Composer, caso não tenha instalado [verifique aqui](https://getcomposer.org) como instalar no seu ambiente de desenvolvimento.

Em seguida, execute o comando abaixo para que fique tudo no lugar.

```sh
composer install
```

## Servidor de desenvolvimento

Você vai precisar ter um servidor local com o PHP 7 configurado para executar o Wpanel. A forma mais simples de fazer isso é utilizando o servido embutido no PHP 7, para isto basta executar o seguinte comando:

```sh
composer run dev
```
Assim o composer vai executar o servidor embutido apontando para a pasta public.

Alternativamente você pode executar um servidor Apache ou NGINX com PHP localmente ou outra ferramenta como XAMMP/LAMP e utilizar vários recursos avançados, como Virtualhosts que não serão abordados neste momento.

**Nota:** Caso opte por usar um servidor nativo ou outra solução, certifique-se de que o servidor esteja apontando para **public/index.php**

## Banco de dados
_* Estes passos são exclusivos para MySQL, caso queira utilizar o SQLIte, siga para o próximo passo._

- Crie um banco de dados no seu MySQL local utilizando o PHPMyAdmin por exemplo ou outra ferramenta de sua preferência.
- Configure as credenciais de conexão do banco de dados no arquivo ```/public/config/config.php``` no array **$db['development']** como demonstrado abaixo:

```php
$db['development'] = array(
    'dsn'	=> '',
    'hostname' => 'localhost',    // Hostname
    'username' => 'root',         // Username
    'password' => 'root',         // Password
    'database' => 'wpanel4-cms',  // DB Name
    'dbdriver' => 'mysqli',       // DB Driver
    'dbprefix' => '',
    'pconnect' => TRUE,
    'db_debug' => (ENVIRONMENT !== 'production'),
    'cache_on' => FALSE,
    'cachedir' => '',
    'char_set' => 'utf8',
    'dbcollat' => 'utf8_general_ci',
    'swap_pre' => '',
    'encrypt' => FALSE,
    'compress' => FALSE,
    'stricton' => FALSE,
    'failover' => array(),
    'save_queries' => TRUE
);
```
Para mais informações sobre as configurações de conexão com bancos de dados [leia a documentação do CodeIgniter aqui](https://codeigniter.com/userguide3/database/configuration.html).

## Execute o Setup do Wpanel

O processo de Setup consiste em criar a estrutura de tabelas no banco de dados utilizando as [migrations](https://codeigniter.com/userguide3/libraries/migration.html), e a criaçao de um usuário ROOT no painel de controle. Este usuário terá os privilégios de Desenvolvedor, que será abordado posteriormente em tópicos avançados.

Acesse o link http://localhost:8080/index.php/setup ou o endereço que você colocou no seru servidor local, ex: http:/seuvirtualhost/index.php/setup

![Setup](https://dotsistemas.com.br/wpanel/setup.png?raw=true)

Preencha o formulário com as credenciais de acesso do usuário e clique em avançar.

Após este processo, você será direcionado para o login da Administração do site.

![Login](https://dotsistemas.com.br/wpanel/login.png?raw=true)

Efetue o login e seja bem vindo!

![Admin](https://dotsistemas.com.br/wpanel/admin.png?raw=true)

Clique na opção "Visualizar site" na barra superior direita para acessar o site.

![Home](https://dotsistemas.com.br/wpanel/home.png?raw=true)

# Publicando em produçao

Publicar seu projeto em produção não é muito diferente dos passos anteriores, exceto que você precisa se atentar para alguns detalhes.

## Enviando os arquivos para o servidor

Você pdoe enviar os arquivos do projeto para seu servidor de produção atraves de FTP ou utilizando o GIT, caso tenha acesso ao terminal do servidor. Verifique essa disponibilidade com seu provedor.

Sabemos que alguns provedores de hospedagem compartilhada utiliza uma pasta pública específica em sua configuração que pode ser **public_html** ou **www**, neste caso você precisará renomear a pasta public do Wpanel para a sua situação.

Ao final a estrutura de arquivos deve estar parecida com isto:

```sh
/path/userfolder
  |- /app
  |- /sys
  |- /public_html   <--- Pasta pública
        |- index.php
        |- .htaccess
```

_**Nota**: Certifique-se de que ao acessar **seudominio.com** o servidor esteja chegando em index.php_

## Banco de dados de produção
_* Estes passos são exclusivos para MySQL, caso queira utilizar o SQLIte, siga para o próximo passo._

- Crie um banco de dados no MySQL do seu servidor utilizando PHPMyAdmin ou outra ferramenta disponibilizada pelo seu provedor de hospedagem.

- Configure as credenciais de conexão do banco de dados no arquivo ```/public/config/config.php``` no array **$db['production']** como demonstrado abaixo:

```php
$db['production'] = array(
    'dsn'	=> '',
    'hostname' => 'localhost',    // Hostname
    'username' => 'root',         // Username
    'password' => 'root',         // Password
    'database' => 'wpanel4-cms',  // DB Name
    'dbdriver' => 'mysqli',       // DB Driver
    'dbprefix' => '',
    'pconnect' => TRUE,
    'db_debug' => (ENVIRONMENT !== 'production'),
    'cache_on' => FALSE,
    'cachedir' => '',
    'char_set' => 'utf8',
    'dbcollat' => 'utf8_general_ci',
    'swap_pre' => '',
    'encrypt' => FALSE,
    'compress' => FALSE,
    'stricton' => FALSE,
    'failover' => array(),
    'save_queries' => TRUE
);
```
Para mais informações sobre as configurações de conexão com bancos de dados [leia a documentação do CodeIgniter aqui](https://codeigniter.com/userguide3/database/configuration.html).

## Mude o Wpanel para o modo de produção

Por padrão o Wpanel é distribuído em modo de desenvolvimento, isso ativa alguns comportamentos específicos no projeto, como o Profiler, mensagens de erro para debug e outras coisas. Ativar o Wpanel em modo de produção é importante para que seu projeto não exiba certas informações para o usuário final.

Para entrar em modo produção, altere a constante ENVIRONMENT para production no arquivo ```/public/index.php``` por volta da linha 76 conforme abaixo.

De:
```php
define('ENVIRONMENT', isset($_SERVER['CI_ENV']) ? $_SERVER['CI_ENV'] : 'development');
```
Para:
```php
define('ENVIRONMENT', isset($_SERVER['CI_ENV']) ? $_SERVER['CI_ENV'] : 'production');
```

## Execute o Setup do Wpanel em produção

O processo de Setup segue o mesmo princípio do processo efetuado no ambiente de desenvolvimento.

Acesse o link http://seusite.com/index.php/setup

![Setup](https://dotsistemas.com.br/wpanel/setup.png?raw=true)

Preencha o formulário com as credenciais de acesso do usuário e clique em avançar.

Após este processo, você será direcionado para o login da Administração do site.

![Login](https://dotsistemas.com.br/wpanel/login.png?raw=true)

Efetue o login e seja bem vindo!

![Admin](https://dotsistemas.com.br/wpanel/admin.png?raw=true)

Clique na opção "Visualizar site" na barra superior direita para acessar o site.

![Home](https://dotsistemas.com.br/wpanel/home.png?raw=true)

_**Nota**: Alternativamente é possível que você restaure um backup do seu banco local no servidor de produção dispensando este passo, porém é altamente recomendável que efetue a manutenção do seu banco de dados utilizando as Migrations por diversos fatores, como versionamento e possibilidade de rollback em caso de erros._
