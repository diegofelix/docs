# Erros & Logging

- [Detalhes de Erros](#error-detail)
- [Exceções HTTP](#http-exceptions)
- [Tratando Erros](#handling-errors)
- [Tratando Erros 404](#handling-404-errors)
- [Logging](#logging)

## Detalhes de erros

Por padrão, dealhes de erros estão ativados para sua aplicação. Isso significa que quando um erro ocorre é exibida uma página com detalhes e uma mensagem do erro. Você pode desativar detalhes de erros configurando a opção `debug` no arquivo `app/config/app.php` para `false`. **É altamente recomendado que você desative os detalhes de erros em um ambiente de produção.**

## Tratando Erros

Por padrão, o arquivo `app/start/global.php` contém um manipulador de erro para todas as exceções:

	App::error(function(Exception $exception)
	{
		Log::error($exception);
	});

Este é o manipulador de erro mais básico. Contudo, você pode especificar mais manipuladores se necessário. Handlers are called based on the type-hint of the Exception they handle. Por exemplo, você pode criar um manipulador que somente manipula instâncias de `RuntimeException`:

	App::error(function(RuntimeException $exception)
	{
		// Handle the exception...
	});

Se um manipulador de exceção retorna uma resposta, aquela resposta será enviada para o browser e nenhum outro manipulador será chamado:

	App::error(function(InvalidUserException $exception)
	{
		Log::error($exception);

		return 'Sorry! Something is wrong with this account!';
	});

Para monitorar erros fatais no PHP, você pode utilizar o método `App::fatal`:

	App::fatal(function($exception)
	{
		//
	});

<a name="http-exceptions"></a>
## Exceções HTTP

Exceções em relação ao HTTP, referem-se a erros que podem ocorrer durante uma solicitação do cliente. Pode ser um erro de página não encontrada (404), um erro de autorização (401) ou até mesmo um erro 500. Para retornar tal resposta, utilize o seguinte:

	App::abort(404, 'Page not found');

O primeiro argumento, é o código de status HTTP, com o seguinte argumento sendo uma mensagem personalizada que você gostaria de mostrar com o erro.

A fim de levantar uma exceção de 401 não autorizado, basta fazer o seguinte:

	App::abort(401, 'You are not authorized.');

Estas exceções podem ser executadas em qualquer momento durante o ciclo de vida da requisição.

<a name="handling-404-errors"></a>
## Tratando Erros 404

Você pode registrar um manipulador de erro que manipula todos os erros de "404 Not Found" da sua aplicação, permitindo retornar páginas de erro 404 personalizadas:

	App::missing(function($exception)
	{
		return Response::view('errors.missing', array(), 404);
	});

<a name="logging"></a>
## Logging

The Laravel logging facilities provide a simple layer on top of the powerful [Monolog](http://github.com/seldaek/monolog). By default, Laravel is configured to create daily log files for your application, and these files are stored in `app/storage/logs`. You may write information to these logs like so:

	Log::info('This is some useful information.');

	Log::warning('Something could be going wrong.');

	Log::error('Something is really going wrong.');

The logger provides the seven logging levels defined in [RFC 5424](http://tools.ietf.org/html/rfc5424): **debug**, **info**, **notice**, **warning**, **error**, **critical**, and **alert**.

Monolog has a variety of additional handlers you may use for logging. If needed, you may access the underlying Monolog instance being used by Laravel:

	$monolog = Log::getMonolog();

You may also register an event to catch all messages passed to the log:

**Registering A Log Listener**

	Log::listen(function($level, $message, $context)
	{
		//
	});
