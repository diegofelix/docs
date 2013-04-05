# Instalação

- [Instalando o Composer](#install-composer)
- [Instalando o Laravel](#install-laravel)
- [Requisitos do Servidor](#server-requirements)
- [Configuração](#configuration)
- [URLs amigáveis](#pretty-urls)

<a name="install-composer"></a>
## Instalando o Composer

Laravel utiliza o [Composer](http://getcomposer.org) para gerenciar suas dependências. Primeiro, baixe uma cópia do `composer.phar`. Uma vez que você tenha o arquivo PHAR, você pode mantê-lo na pasta do seu projeto ou movê-lo para `usr/local/bin` para usá-lo globalmente em seu sistema. No Windows, você pode usar o [Instalador do Windows](https://getcomposer.org/Composer-Setup.exe).

<a name="install-laravel"></a>
## Instalando o Laravel

Com o Composer instalado, faça o download da [última versão](https://github.com/laravel/laravel/archive/develop.zip) do framework Laravel e extraia seu conteúdo em um diretório no seu servidor. A seguir, na raíz da sua aplicação, execute o comando `php composer.phar install` para instalar todas as dependências do framework. Este processo requer que o Git esteja instalado no servidor para completar com sucesso a instalação.

<a name="server-requirements"></a>
## Requisitos do Servidor

O framework Laravel tem poucos requisitos:

- PHP >= 5.3.7
- Extensão MCrypt PHP

<a name="configuration"></a>
## Configuração

Laravel não precisa de quase nenhuma configuração. Você está livre para começar a desenvolver! Entretanto, você pode revisar o arquivo `app/config/app.php` e sua documentação. Este arquivo contém várias opções como `timezone` e `locale` que você pode desejar alterar de acordo com sua aplicação.

> **Nota:** Uma opção de configuração que você deve definir é a opção `key` dentro do arquivo `app/config/app.php`. Este valor deve ser definido como um valor de 32 caracteres, seqüência aleatória. Esta opção é usada para criptografar valores, e valores criptografados não estarão seguros até que sejam devidamente definidos. Você pode definir rapidamente este valor usando o seguinte comando do artisan `php artisan key:generate`.

<a name="permissions"></a>
### Permissões
Laravel requer um conjunto de permissões a ser configurado - pastas dentro de app/storage requerem acesso de escrita pelo servidor.

<a name="paths"></a>
### Caminhos

Vários dos caminhos de diretório do framework são configuráveis. Para mudar o local destes diretórios, confira o arquivo `bootstrap/paths.php`.

<a name="pretty-urls"></a>
## URLs amigáveis

O framework vem com o arquivo `public/.htaccess` que é utilizado para permitir URLs sem `index.php`. Se você usa o Apache na sua aplicação do Laravel, esteja certo de ativar o módulo `mod_rewrite`.

Se o arquivo `.htaccess` que vem com o Laravel não funcionar com sua instalação do Apache, tente este outro:

	Options +FollowSymLinks
	RewriteEngine on

	RewriteCond %{REQUEST_FILENAME} !-f
	RewriteCond %{REQUEST_FILENAME} !-d

	RewriteRule . index.php [L]
