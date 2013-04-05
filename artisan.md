# Artisan CLI

- [Introdução](#introduction)
- [Modo de usar](#usage)

<a name="introduction"></a>
## Introdução

Artisan e o nome da linha de comando da interface incluido junto ao Laravel. O mesmo provem a numero de comandos de helpers para você utilizar enquanto desenvolve sua aplicação. Além de ser o drive do poderoso Symfony Console. 

<a name="usage"></a>
## Modo de usar

Para ver um lista de todos os comandos disponíveis no Artisan, você poderá usar o comando `list`:

**Lista Todos Os Comandos Disponíveis**

	php artisan list

Todos os comandos disponhem de uma ajuda atravêz do comando "help" mostrando assim uma tela com a descrição referente ao comando solicitado e seus argumentos e opções. Para mostrar a tela de ajuda, simplesmente informe o comando com o `help`:

**Vizualizando O A Tela De Ajuda Para O Comando**

	php artisan help migrate

Você pode especificar a configuração do ambiente que devem ser utilizado enquanto está executando um determinado comando utilizamos o comando `--env`:

**Especifica A Configuração Do Ambiente**

	php artisan migrate --env=local

Voce pode também verificar a versão atual do seu Laravel instalado usando a opção `--version`:

**Mostrando A Versão Atual Do Seu Laravel**

	php artisan --version
