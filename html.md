# Forms & HTML

- [Abrindo um formulário](#opening-a-form)
- [Proteção contra CSRF](#csrf-protection)
- [Form Model Binding](#form-model-binding)
- [Labels](#labels)
- [Text, Text Area, Password e Hidden](#text)
- [File Input](#file-input)
- [Checkboxes e Radio Buttons](#checkboxes-and-radio-buttons)
- [Drop-Downs](#drop-down-lists)
- [Buttons](#buttons)
- [Macros personalizadas](#custom-macros)

<a name="opening-a-form"></a>
## Abrindo um formulário

**Abrindo um formulário**

	{{ Form::open(array('url' => 'foo/bar')) }}
		//
	{{ Form::close() }}

Por padrão, será assumido o método `POST`. Porém você é livre para especificar outro método:

	echo Form::open(array('url' => 'foo/bar', 'method' => 'put'))

> **Nota:** Uma vez que formulários HTML suportam apenas `POST`, para os métodos `PUT` e `DELETE` será inserido automaticamente um campo hidden com nome `_method` no seu formulário para simular tal requisição.

Você pode abrir formulários que apontam para rotas nomeadas ou actions de controllers.

	echo Form::open(array('route' => 'route.name'))

	echo Form::open(array('action' => 'Controller@method'))

If your form is going to accept file uploads, add a `files` option to your array:
Se seu formulário for aceitar upload de arquivos, adicione uma opção `files` ao seu array:

	echo Form::open(array('url' => 'foo/bar', 'files' => true))

<a name="csrf-protection"></a>
## Proteção contra CSRF

Laravel provides an easy method of protecting your application from cross-site request forgeries. First, a random token is placed in your user's session. Don't sweat it, this is done automatically. Next, use the token method to generate a hidden form input field containing the random token on your form:

Laravel oferece uma forma fácil de proteger sua aplicação contra CSRF ( cross-site request forgery ). Primeiro, um token aleatório será adicionado na sessão do seu usuário. Não se preocupe, isso é feito automaticamente. Em seguida, use o método token para gerar um input hidden contendo o token no seu formulário.

**Adicionando o Token CSRF em um formulário**

	echo Form::token();

**Adicionar um filtro CSRF à uma rota**

	Route::post('profile', array('before' => 'csrf', function()
	{
		//
	}));

<a name="form-model-binding"></a>
## Form Model Binding

Muitas vezes, você vai querer popular um formulário baseado nos dados de um Model. Para fazer isso use o método `Form::model`:

**Abrindo um Formulário Model**

	echo Form::model($user, array('route' => 'user.update'))

Now, when you generate a form element, like a text input, the model's value matching the field's name will automatically be set as the field value. So, for example, for a text input named `email`, the user model's `email` attribute would be set as the value. However, there's more! If there is an item in the Session flash data matching the input name, that will take precedence over the model's value. So, the priority looks like this:

Agora, quando você gerar um elemento no formulário, como um input text, os valores do model correspondentes aos campos do formulário serão automaticamente setados aos seus respectivos valores. Por exemplo, para um input text chamado `email`, o atributo do User Model `email` será setado no valor do campo. Porém, tem mais! Se há um item na `Flash data` da Sessão correspondente ao nome do input, ele irá prevalecer sobre o valor do Model. Então, a regra é a seguinte:

1. `Flash Data` da Sessão (Input antigo)
2. Valor passado explicitamente
3. Dado do atributo do Model

This allows you to quickly build forms that not only bind to model values, but easily re-populate if there is a validation error on the server!
Isso permite você criar rapidamente formulários que não apenas linkan com os valores dos Models, mas também repopulam caso haja um erro de validação no servidor!

> **Nota:** Quando estiverem usando o `Form::model`, não esqueçam de fechar o formulário usando o `Form::close`!

<a name="labels"></a>
## Labels

**Criando um elemento Label**

	echo Form::label('email', 'E-Mail Address');

**Especificando atributos extras no html**

	echo Form::label('email', 'E-Mail Address', array('class' => 'awesome'));

> **Note:** After creating a label, any form element you create with a name matching the label name will automatically receive an ID matching the label name as well.

> **Nota:** Depois de criar o label, qualquer elemento que você criar que corresponda ao nome do label automaticamente receberá um id com o mesmo nome do label.

<a name="text"></a>
## Text, Text Area, Password & Hidden

**Criando um Input Text**

	echo Form::text('username');

**Especificando um valor padrão**

	echo Form::text('email', 'example@gmail.com');

> **Nota:** Os métodos *hidden* e *textarea*  tem a mesma assinatura do método *text*.

**Criando um input password**

	echo Form::password('password');

<a name="checkboxes-and-radio-buttons"></a>
## Checkboxes e Radio Buttons

**Criando um Input checkbox**

	echo Form::checkbox('name', 'value');

**Criando um checkbox já marcado**

	echo Form::checkbox('name', 'value', true);

> **Nota:** O método *radio* tem a mesma assinatura que o método *checkbox*.

<a name="file-input"></a>
## File Input

**Criando um input file**

	echo Form::file('image');

<a name="drop-down-lists"></a>
## Drop-Downs

**Generating A Drop-Down List**

	echo Form::select('size', array('L' => 'Large', 'S' => 'Small'));

**Generating A Drop-Down List With Selected Default**

	echo Form::select('size', array('L' => 'Large', 'S' => 'Small'), 'S');

<a name="buttons"></a>
## Buttons

**Criando um Button submit**

	echo Form::submit('Click Me!');

> **Nota:** Precisa criar um elemento do tipo button? Tente o método *button*. Ele tem a mesma assinatura do *submit*.

<a name="custom-macros"></a>
## Macros personalizadas

É fácil definir seus próprios elementos chamados de "macros". Veja como funciona. Primeiro, simplesmente registre a macro com o seu devido nome e uma Closure.

**Registrando uma Macro de formulário**

	Form::macro('myField', function()
	{
		return '<input type="awesome">';
	});

Agora você pode chamar sua macro usando o seu nome:

**Chamando uma macro personalizada**

	echo Form::myField();
