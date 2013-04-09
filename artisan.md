# Artisan CLI

- [Introdução](#introduction)
- [Modo de usar](#usage)

<a name="introduction"></a>
## Introdução

Artisan é o nome da interface de linha de comando incluída no Laravel. Fornece um número de comandos úteis para seu uso durante o desenvolvimento de sua aplicação. É conduzido pelo poderoso componente Symfony Console.

<a name="usage"></a>
## Modo de usar

Para ver a lista com todos os comandos do Artisan, você pode usar o comando `list`:

**Listando todos os comandos disponíveis**

	php artisan list

Todo comando inclui uma tela de ajuda que exibe e descreve os argumentos e opções disponíveis deste comando. Para exibir a tela de ajuda, simplesmente preceda o nome do comando com `help`:

**Exibindo a tela de ajuda para um comando**

	php artisan help migrate

Você pode especificar o ambiente de configuração que devem ser utilizados durante a execução de um comando usando a chave `--env`:

**Especificando o ambiente de configuração**

	php artisan migrate --env=local

Você pode ver a versão atual da sua instalação do Laravel usando a opção `--version`:

**Exibindo sua versão atual do Laravel**

	php artisan --version
