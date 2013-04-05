# E-mail

- [Configuração](#configuration)
- [Uso básico](#basic-usage)
- [Incorporando anexos inline](#embedding-inline-attachments)

<a name="configuration"></a>
## Configuração

Laravel provê uma API limpa e simples sobre a popular biblioteca [SwiftMailer](http://swiftmailer.org). O arquivo de configuração de email é `app/config/mail.php`, e contém opções que lhe permitem alterar seu servidor SMTP, porta, e credenciais, bem como definir um endereço global `from` para todas as mensagens entregues pela biblioteca. Você pode usar o servidor SMTP que desejar. Se você deseja usar a função `mail` do PHP para enviar email, você pode mudar o `driver` para `mail` no arquivo de configuração.

<a name="basic-usage"></a>
## Uso básico

O método `Mail::send` pode ser usado para enviar uma mensagem de e-mail:

	Mail::send('emails.welcome', $data, function($m)
	{
		$m->to('foo@example.com', 'John Smith')->subject('Welcome!');
	});

O primeiro argumento passado para o método `send` é o nome da view que que deverá ser usada como o corpo do e-mail. O segundo é o `$data` que deverá ser passado para a view, e o terceiro é o Closure permitindo que você especifique várias opções na mensagem de e-mail.

> **Note:** A `$message` variable is always passed to e-mail views, and allows the inline embedding of attachments. So, it is best to avoid passing a `message` variable in your view payload.

Você também pode especificar uma view de texto simples para usar em complemento com uma view HTML:

	Mail::send(array('html.view', 'text.view'), $data, $callback);

Ou, você pode especificar somente um tipo de view usando as chaves `html` ou `text`:

	Mail::send(array('text' => 'view'), $data, $callback);

Você pode especificar outras opções na mensagem de e-mail como outras cópias ou anexos bem como:

	Mail::send('emails.welcome', $data, function($m)
	{
		$m->from('us@example.com', 'Laravel');

		$m->to('foo@example.com')->cc('bar@example.com');

		$m->attach($pathToFile);
	});

Ao anexar arquivos para uma mensagem, você pode especificar também um tipo MIME e / ou um nome de exibição:

	$m->attach($pathToFile, array('as' => $display, 'mime' => $mime));

> **Note:** The message instance passed to a `Mail::send` Closure extends the SwiftMailer message class, allowing you to call any method on that class to build your e-mail messages.

<a name="embedding-inline-attachments"></a>
## Incorporando anexos inline

Incorporar imagens inline em seus e-mails é normalmente complicado; Entretando, Laravel provê uma forma conveniente de anexar imagens em seus e-mails e recuperar o CID apropriado.

**Incorporando uma imagem em uma view de E-Mail**

	<body>
		Here is an image:

		<img src="<?php echo $message->embed($pathToFile); ?>">
	</body>

**Incorporando Raw Data em uma view de E-Mail**

	<body>
		Here is an image from raw data:

		<img src="<?php echo $message->embedData($data, $name); ?>">
	</body>

Note que a variável `$message` é sempre passada para a view de e-mail pela classe `Mail`.
