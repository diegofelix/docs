# Controladores

- [Controladores Básicos](#basic-controllers)
- [Filtros de Controladores](#controller-filters)
- [Controladores RESTful](#restful-controllers)
- [Controladores de Recursos](#resource-controllers)
- [Resolvendo Métodos Ausentes](#handling-missing-methods)

<a name="basic-controllers"></a>
## Controladores Básicos

Em vez de definir toda a sua lógica de rotas no arquivo de `routes.php`, você pode querer organizar este comportamento usando classes Controladores. Controladores podem agrupar a lógica relacionada a rota em uma classe, além de tirar vantagem de recursos mais avançados em Laravel, tais como [injeção de dependência](/docs/ioc) automática.

Os controladores são normalmente armazenados no diretório `app/controllers`, e este diretório já vem predefinidp na opção `classmap` de seu arquivo `composer.json`.

Abaixo segue um exemplo de uma classe de Controlador Básico:


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

Todos controladores deve extender a classe `BaseController`. O `BaseController` é armazenado no diretório `app/controllers`, onde a lógica compartilhada em controladores pode ser colocada. A classe `BaseController` extende a classe `Controller` da framework. Agora podemos encaminhar a ação do controlador:

	Route::get('user/{id}', 'UserController@showProfile');

Se você escolher organizar seu controlador usando PHP namespaces, basta usar o nome da classe com o namespace correspondente para definir a rota:

	Route::get('foo', 'Namespace\FooController@method');

Você também pode especificar nomes em rotas de controlador:

	Route::get('foo', array('uses' => 'FooController@method',
											'as' => 'name'));

Para gerar uma URL para uma ação de controlador, você pode usar o método `URL::action`:

	$url = URL::action('FooController@method');

Você pode acessar o nome da ação do controlador que está sendo executado com o método `currentRouteAction`:

	$action = Route::currentRouteAction();

<a name="controller-filters"></a>
## Filtros de Controladores

Os [filtros](/docs/routing#route-filters) podem ser especificados em rotas controladoras semelhantes às rotas regulares:

	Route::get('profile', array('before' => 'auth',
				'uses' => 'UserController@showProfile'));

No entanto, você também pode especificar filtros de dentro de seu controlador:

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

Você também pode especificar filtros de controladorers usando funções anônimas (Closures):

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
## Controladores RESTful

Laravel permite que você facilmente defina uma única rota para lidar com cada ação em um controlador usando simples, convenções de nomenclatura de REST. Primeiro defina uma rota usando o método `Route::controller`:


**Definindo um Controlador RESTful**

	Route::controller('users', 'UserController');

O método `controller` aceita dois argumentos. O primeiro é a URI base que controlador manipula, enquanto o segundo é o nome da classe do controlador. Em seguida, basta adicionar métodos em seu controlador, prefixado com o verbo HTTP que eles respondem:

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

O método `index` irá responder ao URI raiz implementado pelo controlador que, neste caso, é `users`.

Se a ação do sue controlador contém múltiplas palavras, você pode acessar a ação usando traços na sintaxe na URI. Por exemplo, a ação de controlador no nosso `UserController` responderia a URI `users/admin-profile` da seguinte forma:

	public function getAdminProfile() {}

<a name="resource-controllers"></a>
## Controladores de Recursos

Controladores de recursos tornam mais fácil para construir controladores RESTful em torno de recursos. Por exemplo, você pode desejar criar um controlador que gerencia "photos" armazenadas pelo seu aplicativo. Usando o comando `controller:make` através do Artisan CLI e o método `Route::resource`, podemos criar rapidamente tal controlador.

Para criar o controlador via linha de comando, execute o seguinte comando:

	php artisan controller:make PhotoController

Agora podemos registar uma rota de recursos para o controlador:

	Route::resource('photo', 'PhotoController');

This single route declaration creates multiple routes to handle a variety of RESTful actions on the photo resource. Likewise, the generated controller will already have stubbed methods for each of these actions with notes informing you which URIs and verbs they handle.
Esta única declaração cria múltiplas rotas para lidar com uma variedade de ações RESTful no recurso de "photos". Da mesma forma, o controlador gerado já terá métodos preparados para cada uma dessas ações com anotações informando quais URIs e verbos lidam com eles.


**Ações tratadas por Controlador de Recursos**

Verso     | Caminho               | Ação
----------|-----------------------|--------------
GET       | /resource             | index
GET       | /resource/create      | create
POST      | /resource             | store
GET       | /resource/{id}        | show
GET       | /resource/{id}/edit   | edit
PUT/PATCH | /resource/{id}        | update
DELETE    | /resource/{id}        | destroy

Às vezes, você só precisa lidar com um subconjunto das ações de recursos:

	php artisan controller:make PhotoController --only=index,show

	php artisan controller:make PhotoController --except=index

Você também pode especificar um subconjunto de ações para lidar com a rota:

	Route::resource('photo', 'PhotoController',
					array('only' => array('index', 'show')));

<a name="handling-missing-methods"></a>
## Resolvendo Métodos Ausentes

A catch-all method may be defined which will be called when no other matching method is found on a given controller. The method should be named `missingMethod`, and receives the parameter array for the request as its only argument:

A pega-tudo método pode ser definido, que será chamada quando nenhum outro correspondente método é encontrado em um determinado controlador. O método deve ser chamado `missingMethod`, e recebe "the parameter array for the request" como seu único argumento:

**Definindo um Pega-Tudo Método**

	public function missingMethod($parameters)
	{
		//
	}
