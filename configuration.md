# Configuração

- [Introdução](#introduction)
- [Configuração de ambiente](#environment-configuration)

<a name="introduction"></a>
## Introdução

Todos os arquivos de configuração do Laravel estão armazenados no diretório `app/config`. Cada opção está documentada em seu respectivo arquivo, fique a vontade para olhar os arquivos e se familiarizar com as opções disponíveis para você.

Em algum momento você pode precisar acessar os valores de configuração em tempo de execução. Você pode fazer isso usando a classe `Config`:

**Acessando um valor de configuração**

	Config::get('app.timezone');

Note que o estilo de sintaxe "ponto" pode ser usado para acessar valores em vários arquivos. Você também pode definir valores de configuração em tempo de execução:

**Definindo um valor de configuração**

	Config::set('database.default', 'sqlite');

<a name="environment-configuration"></a>
## Configuração de ambiente

Muitas vezes é útil ter valores diferentes com base no ambiente onde o aplicativo é executado. Por exemplo, você pode querer usar um drive de cache em sua maquina de desenvolvimento local diferente do drive usado em seu servidor de produção. É fácil fazer isto usando configuração baseada no ambiente.

Simplismente crie uma pasta dentro do diretório `config` que corresponde ao seu nome de ambiente, por exemplo `local`. Em seguida, crie os arquivos de configuração que você deseja sobrescrever e especifique as opções para este ambiente. Por exemplo, para sobrescrever o driver de cache para seu ambiente local, você poderia criar um arquivo `cache.php` em `app/config/local` com o seguinte conteúdo:

	<?php

	return array(

		'driver' => 'file',

	);

> **Note:** Não use 'testing' como um nome de ambiente. Esta é reservada para testes de unidade.

Observe que você não precisa especificar a opção _every_ que esta no arquivo base de configuração, mas somente as opções que você deseja sobrescrever. The environment configuration files will "cascade" over the base files.

Em seguida, nós precisamos instruir o framework como detectar em qual ambiente ele será executado. O ambiente padrão é sempre `production`. Todavia, você pode definir outro ambiente no arquivo `bootstrap/start.php` na raiz de sua instalação. Neste arquivo você encontrará uma chamada para `$app->detectEnvironment`. O array passado para este método é usado para determinar o ambiente atual. Você pode adicionar outros ambientes e nomes de maquina para o array conforme precisar.

    <?php

    $env = $app->detectEnvironment(array(

        'local' => array('your-machine-name'),

    ));

Você também pode passar uma `Closure` para o método `detectEnvironment`, permitindo que você implemente sua própria detecção de ambiente:

	$env = $app->detectEnvironmnet(function()
	{
		return $_SERVER['MY_LARAVEL_ENV'];
	});

Você pode acessar o ambiente atual da aplicação pelo método `environment`:

**Acessando o ambiente atual da aplicação**

	$environment = App::environment();
