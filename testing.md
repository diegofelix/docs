# Unit Testing

- [Introduction](#introduction)
- [Defining & Running Tests](#defining-and-running-tests)
- [Test Environment](#test-environment)
- [Calling Routes From Tests](#calling-routes-from-tests)
- [Mocking Facades](#mocking-facades)
- [Framework Assertions](#framework-assertions)
- [Helper Methods](#helper-methods)

<a name="introduction"></a>
## Introdução

O Laravel foi construido com testes unutários em mente. Portanto isso há suporte para testes com PHPUnit é incluido out of the box, e um arquivo `phpunit.xml` que já está configurado para uso com sua aplicação. Além PHPUnit, o Laravel também utiliza o Symfony HttpKernel, DomCrawler, componentes BrowserKit para lhe permitir inspecionar e manipular suas views enquanto às testa, permitindo simular um navegador da web.

Um arquivo exemplo de teste é fornecido no diretório `app/tests`. Depois de instalar uma nova aplicação laravel, simplesmente inicie o  `phpunit` na linha de comando para iniciar seus testes.

<a name="defining-and-running-tests"></a>
## Definindo e executando Testes

To create a test case, simply create a new test file in the `app/tests` directory. The test class should extend `TestCase`. You may then define test methods as you normally would when using PHPUnit.

**An Example Test Class**

	class FooTest extends TestCase {

		public function testSomethingIsTrue()
		{
			$this->assertTrue(true);
		}

	}

You may run all of the tests for your application by executing the `phpunit` command from your terminal.

> **Note:** If you define your own `setUp` method, be sure to call `parent::setUp`.

<a name="test-environment"></a>
## Test Environment

When running unit tests, Laravel will automatically set the configuration environment to `testing`. Also, Laravel includes configuration files for `session` and `cache` in the test environment. Both of these drivers are set to `array` while in the test environment, meaning no session or cache data will be persisted while testing. You are free to create other testing environment configurations as necessary.

<a name="calling-routes-from-tests"></a>
## Calling Routes From Tests

You may easily call one of your routes for a test using the `call` method:

**Calling A Route From A Test**

	$response = $this->call('GET', 'user/profile');

	$response = $this->call($method, $uri, $parameters, $files, $server, $content);

You may then inspect the `Illuminate\Http\Response` object:

	$this->assertEquals('Hello World', $response->getContent());

You may also call a controller from a test:

**Calling A Controller From A Test**

	$response = $this->action('GET', 'HomeController@index');

	$response = $this->action('GET', 'UserController@profile', array('user' => 1));

The `getContent` method will return the evaluated string contents of the response. If your route returns a `View`, you may access it using the `original` property:

	$view = $response->original;

	$this->assertEquals('John', $view['name']);

To call a HTTPS route, you may use the `callSecure` method:

	$response = $this->callSecure('GET', 'foo/bar');

### DOM Crawler

You may also call a route and receive a DOM Crawler instance that you may use to inspect the content:

	$crawler = $this->client->request('GET', '/');

	$this->assertTrue($this->client->getResponse()->isOk());

	$this->assertCount(1, $crawler->filter('h1:contains("Hello World!")'));

For more information on how to use the crawler, refer to its [official documentation](http://symfony.com/doc/master/components/dom_crawler.html).

<a name="mocking-facades"></a>
## Mocking Facades

When testing, you may often want to mock a call to a Laravel static facade. For example, consider the following controller action:

	public function getIndex()
	{
		Event::fire('foo', array('name' => 'Dayle'));

		return 'All done!';
	}

We can mock the call to the `Event` class by using the `shouldReceive` method on the facade, which will return an instance of a [Mockery](https://github.com/padraic/mockery) mock.

**Mocking A Facade**

	public function testGetIndex()
	{
		Event::shouldReceive('fire')->once()->with(array('name' => 'Dayle'));

		$this->call('GET', '/');
	}

> **Note:** You should not mock the `Request` facade. Instead, pass the input you desire into the `call` method when running your test.

<a name="framework-assertions"></a>
## Framework Assertions

Laravel ships with several `assert` methods to make testing a little easier:

**Asserting Responses Are OK**

	public function testMethod()
	{
		$this->call('GET', '/');

		$this->assertResponseOk();
	}

**Asserting Responses Are Redirects**

	$this->assertRedirectedTo('foo');

	$this->assertRedirectedToRoute('route.name');

	$this->assertRedirectedToAction('Controller@method');

**Asserting A View Has Some Data**

	public function testMethod()
	{
		$this->call('GET', '/');

		$this->assertViewHas('name');
		$this->assertViewHas('age', $value);
	}

**Asserting The Session Has Some Data**

	public function testMethod()
	{
		$this->call('GET', '/');

		$this->assertSessionHas('name');
		$this->assertSessionHas('age', $value);
	}

<a name="helper-methods"></a>
## Helper Methods

The `TestCase` class contains several helper methods to make testing your application easier.

You may set the currently authenticated user using the `be` method:

**Setting The Currently Authenticated User**

	$user = new User(array('name' => 'John'));

	$this->be($user);

You may re-seed your database from a test using the `seed` method:

**Re-Seeding Database From Tests**

	$this->seed();

	$this->seed($connection);

More information on creating seeds may be found in the [migrations and seeding](/docs/migrations#database-seeding) section of the documentation.
