# E-mail

- [Configuração](#configuration)
- [Uso básico](#basic-usage)
- [Incorporando anexos inline](#embedding-inline-attachments)

<a name="configuration"></a>
## Configuração

Laravel oferece uma API limpa e simples sobre a popular biblioteca [SwiftMailer](http://swiftmailer.org). O arquivo de configuração de email é `app/config/mail.php`, e contém opções que lhe permitem alterar seu servidor SMTP, porta, e credenciais, bem como definir um endereço global `from` para todas as mensagens entregues pela biblioteca. Você pode usar o servidor SMTP que desejar. Se você deseja usar a função `mail` do PHP para enviar email, você pode mudar o `driver` para `mail` no arquivo de configuração.

<a name="basic-usage"></a>
## Uso básico

O método `Mail::send` pode ser usado para enviar uma mensagem de e-mail:

	Mail::send('emails.welcome', $data, function($m)
	{
		$m->to('foo@example.com', 'John Smith')->subject('Welcome!');
	});

O primeiro argumento passado para o método `send` é o nome da view que deverá ser usada como o corpo do e-mail. O segundo é o `$data` que deverá ser passado para a view, e o terceiro é o Closure permitindo que você especifique várias opções na mensagem de e-mail.

> **Nota:** A variavel `$message` é sempre passada para as views de email, e permite a incorporação de anexos. Então, é melhor evitar passar uma variavel `message` para sua view.

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

> **Nota:** A instância da mensagem passada para a Closure `Mail::send` estende a classe de mensagem do SwiftMailer, permitindo que você chame qualquer método existente nesta classe para construir seus e-mails.

<a name="embedding-inline-attachments"></a>
## Incorporando anexos inline

Incorporar imagens inline em seus e-mails é normalmente complicado; Entretando, Laravel provê uma forma conveniente de anexar imagens em seus e-mails e recuperar o CID apropriado.

**Incorporando uma imagem em uma view de e-mail**

	<body>
		Here is an image:

		<img src="<?php echo $message->embed($pathToFile); ?>">
	</body>

**Incorporando Raw Data em uma view de e-mail**

	<body>
		Here is an image from raw data:

		<img src="<?php echo $message->embedData($data, $name); ?>">
	</body>

Note que a variável `$message` é sempre passada para a view de e-mail pela classe `Mail`.