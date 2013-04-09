# Controllers

- [Controllers Básicos](#basic-controllers)
- [Filtros de Controllers](#controller-filters)
- [Controllers RESTful](#restful-controllers)
- [Resource Controllers](#resource-controllers)
- [Resolvendo Métodos não encontrados](#handling-missing-methods)

<a name="basic-controllers"></a>
## Controllers Básicos

Em vez de definir toda a sua lógica de rotas no arquivo `routes.php`, você pode querer organizar este comportamento usando Controllers. Controllers podem agrupar a lógica relacionada a rota em uma classe, além de tirar vantagem de recursos mais avançados em Laravel, tais como [injeção de dependência](/docs/ioc) automática.

Os Controllers são normalmente armazenados no diretório `app/controllers`, e este diretório já vem predefinido na opção `classmap` de seu arquivo `composer.json`.

Abaixo segue um exemplo de um Controller Básico:


	class UserController extends BaseController {

		/**
		 * Mostrar o perfil de um usuário.
		 */
		public function showProfile($id)
		{
			$user = User::find($id);

			return View::make('user.profile', array('user' => $user));
		}

	}

Todo Controller deve estender a classe `BaseController`. O `BaseController` é armazenado no diretório `app/controllers`, onde a lógica compartilhada entre Controllers pode ser colocada. A classe `BaseController` estende a classe `Controller` do framework. Agora podemos encaminhar a action do Controller:

	Route::get('user/{id}', 'UserController@showProfile');

Se você escolher organizar seu Controller usando namespaces, basta usar o nome da classe com o namespace correspondente para definir a rota:

	Route::get('foo', 'Namespace\FooController@method');

Você também pode especificar nomes em rotas de Controller:

	Route::get('foo', array('uses' => 'FooController@method',
											'as' => 'name'));

Para gerar uma URL para uma action de um Controller, você pode usar o método `URL::action`:

	$url = URL::action('FooController@method');

Você pode acessar o nome da action do Controller que está sendo executado com o método `currentRouteAction`:

	$action = Route::currentRouteAction();

<a name="controller-filters"></a>
## Filtros de Controllers

Os [filtros](/docs/routing#route-filters) podem ser especificados em rotas para Controller semelhantes às rotas regulares:

	Route::get('profile', array('before' => 'auth',
				'uses' => 'UserController@showProfile'));

No entanto, você também pode especificar filtros de dentro de seu Controller:

	class UserController extends BaseController {

		/**
		 * Instanciar uma nova instância de UserController.
		 */
		public function __construct()
		{
			$this->beforeFilter('auth');

			$this->beforeFilter('csrf', array('on' => 'post'));

			$this->afterFilter('log', array('only' =>
								array('fooAction', 'barAction')));
		}

	}

Você também pode especificar filtros de Controllers usando Closures:

	class UserController extends BaseController {

		/**
		 * Instanciar uma nova instância de UserController.
		 */
		public function __construct()
		{
			$this->beforeFilter(function()
			{
				//
			});
		}

	}

<a name="restful-controllers"></a>
## Controllers RESTful

Laravel permite que você defina facilmente uma única rota para lidar com cada action em um Controller usando simples convenções de nomenclatura de REST. Primeiro defina uma rota usando o método `Route::controller`:


**Definindo um Controller RESTful**

	Route::controller('users', 'UserController');

O método `controller` aceita dois argumentos. O primeiro é a URI base que o Controller manipulará, enquanto o segundo é o nome da classe do Controller. Em seguida, basta adicionar métodos em seu Controller, prefixado com o verbo HTTP que eles respondem:

	class UserController extends BaseController {

		public function getIndex()
		{
			//
		}

		public function postProfile()
		{
			//
		}

	}

O método `index` irá responder ao URI raiz implementado pelo Controller que, neste caso, é `users`.

Se a action do seu Controller contém múltiplas palavras, você pode acessar a Action usando "-" ( hífen ) na sintaxe na URI. Por exemplo, a Action de Controller no nosso `UserController` responderia a URI `users/admin-profile` da seguinte forma:

	public function getAdminProfile() {}

<a name="resource-controllers"></a>
## Resources Controllers

Resource Controllers tornam mais fácil para construir Controllers RESTful em torno de recursos. Por exemplo, você pode desejar criar um Controller que gerencia "photos" armazenadas pelo seu aplicativo. Usando o comando `controller:make` através do Artisan CLI e o método `Route::resource`, podemos criar rapidamente tal Controller.

Para criar o Controller via linha de comando, execute o seguinte comando:

	php artisan controller:make PhotoController

Agora podemos registar uma rota de recursos para o Controller:

	Route::resource('photo', 'PhotoController');

Esta única declaração cria múltiplas rotas para lidar com uma variedade de ações RESTful no recurso de "photos". Da mesma forma, o Controller gerado já terá métodos preparados para cada uma dessas ações com anotações informando quais URIs e verbos lidam com eles.


**Ações tratadas por Controller de Recursos**

Verso     | Caminho               | Action
----------|-----------------------|--------------
GET       | /resource             | index
GET       | /resource/create      | create
POST      | /resource             | store
GET       | /resource/{id}        | show
GET       | /resource/{id}/edit   | edit
PUT/PATCH | /resource/{id}        | update
DELETE    | /resource/{id}        | destroy

As vezes, você só precisa lidar com um subconjunto das ações de recursos:

	php artisan controller:make PhotoController --only=index,show

	php artisan controller:make PhotoController --except=index

Você também pode especificar um subconjunto de ações para lidar com a rota:

	Route::resource('photo', 'PhotoController',
					array('only' => array('index', 'show')));

<a name="handling-missing-methods"></a>
## Resolvendo Métodos não encontrados

É um método que será chamado quando nenhum método é encontrado em um determinado Controller. Esse método deve ser chamado de `missingMethod`, e recebe uma array de parâmetros como único argumento.

**Definindo um método "pega-tudo"**

	public function missingMethod($parameters)
	{
		//
	}
