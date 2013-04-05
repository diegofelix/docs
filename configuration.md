# Configuração

- [Introdução](#introduction)
- [Configuração do Ambiente](#environment-configuration)

<a name="introduction"></a>
## Introdução

Todos os arquivos de configuração do Laravel framework estão armazenados no diretório `app/config`. Cada opção de todos os arquivos estão documentados, então sinta-se a vontade para olhar os arquivos e se famializar com as opçãos disponíveis para você.

As vezes você terá necessidade de acessar algum valor em run-time. Voce poderá fazer isso utilizando-se da class `Config`:

**Acessando Um Valor Da Configuração**

	Config::get('app.timezone');

Nota-se que "ponto ( . )" é o estilo da sintaxe pode ser usado para acessar os valores de varios arquivos. Voce também poderá setar um valor na configuração em run-time:

**Setando Um Valor De Configuração**

	Config::set('database.default', 'sqlite');

<a name="environment-configuration"></a>
## Configuração de Ambiente

Muitas vezes é útil ter diferente valores de configurações baseado em um ambiente de aplicação que estamos rodando. Por exemplo, você pode querer utilizar um driver de cache diferente para o ambiente local de desenvolvimento que de um servidor de produção. Isto é fácil para acoplar usando configuração baseado em um ambiente.

Simples criando uma pasta dentro do diretorio `config` que combina com o nome do seu ambiente, semelhante a `local`. Próximo, criar o arquivo de configuração que você deseja sobrescrever e ou especificar as opções do ambiente. Por exemplo, para sobrescrever o caminho do cache para o ambiente `local`, voce deverá criar o arquivo `cache.php` em `app/config/local` com o seguinte conteúdo:

	<?php

	return array(

		'driver' => 'file',

	);

> **Nota:** Não utilize o nome `testing` para nome de ambientes. Pois esté nome é reservado para testes unitários (PHPUnit).

Observe que você não precisa especificar _todas_ as opções que está no arquivo de configuração base, mais somente as opções que você deseja alterar. Os arquivos de configuração do ambiente são carregados "em cascata" sobre o arquivo base

Em seguida, presisamos instruir o quadro para o framework em qual ambiente está sendo executado. Por padrão o ambiente é `production`. Contudo, voce pode configurar outro ambiente dentro do arquivo root `bootstrap/start.php` da sua instalação. Neste arquivo você deverá procurar pela chamada `$app->detectEnvironment`. E então passar um array para o metodo que vocês está usando um determinado ambiente. Você pode adicionar outros ambiente e nomes para maquinas para o array conforme necessário.

    <?php

    $env = $app->detectEnvironment(array(

        'local' => array('nome-da-sua-maquina'),

    ));

Você também pode passar um `Closure` para o metodo `detectEvironment`, permitindo que você implemente o sua próprio forma de detectar o ambiente:

	$env = $app->detectEnvironmnet(function()
	{
		return $_SERVER['MY_LARAVEL_ENV'];
	});

Você pode acessar o ambiente atual da aplicação pelo metodo `environment`:

**Acessando O Ambiente Atual da Aplicação**

	$environment = App::environment();
