# Validation

- [Uso básico](#basic-usage)
- [Trabalhando com mensagens de erro](#working-with-error-messages)
- [Mensagens de erros & Views](#error-messages-and-views)
- [Regras de validação disponíves](#available-validation-rules)
- [Mensagens de erros personalizadas](#custom-error-messages)
- [Regras de validação personalizadas](#custom-validation-rules)

<a name="basic-usage"></a>
## Uso básico

Laravel vem com uma simples, conveniente facilidade para validar dados e retornar mensagens de erro de validação através da classe `Validation`.

**Exemplo básico de validação**

	$validator = Validator::make(
		array('name' => 'Dayle'),
		array('name' => 'required|min:5')
	);

O primeiro argumento passado para o método `make` são os dados a serem validados. O segundo argumento é a regra de validação que deverá ser aplicada a estes dados.

Várias regras podem ser delimitadas utilizando ou o caracter "pipe", ou um array de elementos separados.

**Utilizando arrays para especificar regras**

	$validator = Validator::make(
		array('name' => 'Dayle'),
		array('name' => array('required', 'min:5'))
	);

Uma vez que a instância do `Validator` foi criada, o método `fails` (ou `passes`) pode ser usado para realizar a validação.

	if ($validator->fails())
	{
		// The given data did not pass validation
	}

Se a validação falhar, você pode recuperar as mensagens de erro do validador.

	$messages = $validator->messages();

**Validando arquivos**

A classe `Validator` fornece várias regras para validar arquivos, como 'tamanho', 'mimes' e outros. Ao validar arquivos, você pode simplesmente passá-los para o validador com os outros dados.

<a name="working-with-error-messages"></a>
## Trabalhando com mensagens de erro

Após chamar o método `messages` na instância do `Validator`, você irá receber uma instância da classe `MessageBag`, que tem uma variedade de métodos para trabalhar com mensagens de erros.

**Recuperando a primeira mensagem de erro de um campo**

	echo $messages->first('email');

**Recuperando todas as mensagens de erros de um campo**

	foreach ($messages->get('email') as $message)
	{
		//
	}

**Recuperando todas mensagens de erros de todos os campos**

	foreach ($messages->all() as $message)
	{
		//
	}

**Verificando se uma mensagem de um campo existe**

	if ($messages->has('email'))
	{
		//
	}

**Recuperando uma mensagem de erro com formatação**

	echo $messages->first('email', '<p>:message</p>');

> **Nota:** Por padrão, mensagens são formatadas utilizando a sintaxe compatível com o Bootstrap.

**Recuperando todas as mensagens de erros com formatação**

	foreach ($messages->all('<li>:message</li>') as $message)
	{
		//
	}

<a name="error-messages-and-views"></a>
## Mensagens de erros & Views

Assim que você realizar a validação, você irá precisar de uma maneira fácil de passar as mensagens de erros para suas views. Isto é convenientemente tratado pelo Laravel. Considere as seguintes rotas como exemplo:

	Route::get('register', function()
	{
		return View::make('user.register');
	});

	Route::post('register', function()
	{
		$rules = array(...);

		$validator = Validator::make(Input::all(), $rules);

		if ($validator->fails())
		{
			return Redirect::to('register')->withErrors($validator);
		}
	});

Observe que quando a validação falha, nós passamos a instância `Validator` para o método Redirect utilizando o método `withErrors`. This method will flash the error messages to the session so that they are available on the next request.

However, notice that we do not have to explicitly bind the error messages to the view in our GET route. This is because Laravel will always check for errors in the session data, and automatically bind them to the view if they are available. **So, it is important to note that an `$errors` variable will always be available in all of your views, on every request**, allowing you to conveniently assume the `$errors` variable is always defined and can be safely used. The `$errors` variable will be an instance of `MessageBag`.

So, after redirection, you may utilize the automatically bound `$errors` variable in your view:

	<?php echo $errors->first('email'); ?>

<a name="available-validation-rules"></a>
## Regras de validação disponíves

Abaixo está a lista de todas as regras de validação disponíves e suas funções:

- [Accepted](#rule-accepted)
- [Active URL](#rule-active-url)
- [After (Date)](#rule-after)
- [Alpha](#rule-alpha)
- [Alpha Dash](#rule-alpha-dash)
- [Alpha Numeric](#rule-alpha-num)
- [Before (Date)](#rule-before)
- [Between](#rule-between)
- [Confirmed](#rule-confirmed)
- [Date](#rule-date)
- [Date Format](#rule-date-format)
- [Different](#rule-different)
- [E-Mail](#rule-email)
- [Exists (Database)](#rule-exists)
- [Image (File)](#rule-image)
- [In](#rule-in)
- [Integer](#rule-integer)
- [IP Address](#rule-ip)
- [Max](#rule-max)
- [MIME Types](#rule-mimes)
- [Min](#rule-min)
- [Not In](#rule-not-in)
- [Numeric](#rule-numeric)
- [Regular Expression](#rule-regex)
- [Required](#rule-required)
- [Required With](#rule-required-with)
- [Same](#rule-same)
- [Size](#rule-size)
- [Unique (Database)](#rule-unique)
- [URL](#rule-url)

<a name="rule-accepted"></a>
#### accepted

O campo sob validação deve ser _yes_, _on_, ou _1_. Útil para validação de aceitação de "Termos de Serviço".

<a name="rule-active-url"></a>
#### active_url

O campo sob validação deve ser uma URL válida de acordo com a função `checkdnsrr` do PHP.

<a name="rule-after"></a>
#### after:_date_

O campo sob validação deve ser um valor depois de uma determinada data. As datas serão passadas para a função `strtotime` do PHP.

<a name="rule-alpha"></a>
#### alpha

O campo sob validação deve ser inteiramente formado por caracteres do alfabeto.

<a name="rule-alpha-dash"></a>
#### alpha_dash

O campo sob validação pode ter caracteres alfa-numéricos, bem como traços e underscores.

<a name="rule-alpha-num"></a>
#### alpha_num

O campo sob validação deve ser inteiramente formado por caracteres alfa-numéricos.

<a name="rule-before"></a>
#### before:_date_

O campo sob validação deve ser um valor que antecede uma determinada data. As datas serão passadas para a função `strtotime` do PHP.

<a name="rule-between"></a>
#### between:_min_,_max_

O campo sob validação deve ter um tamanho entre _min_ e _max_. Strings, numéricos, e arquivos são validados da mesma forma como a regra `size`.

<a name="rule-confirmed"></a>
#### confirmed

The field under validation must have a matching field of `foo_confirmation`. For example, if the field under validation is `password`, a matching `password_confirmation` field must be present in the input.

<a name="rule-date"></a>
#### date

O campo sob validação deve ser uma data válida de acordo com a função `strtotime` do PHP.

<a name="rule-date-format"></a>
#### date_format:_format_

The field under validation must match the _format_ defined according to the `date_parse_from_format` PHP function.

<a name="rule-different"></a>
#### different:_field_

O campo _field_ deve ser diferente do campo sob validação.

<a name="rule-email"></a>
#### email

O campo sob validação deve ser formatado como um endereço de e-mail.

<a name="rule-exists"></a>
#### exists:_table_,_column_

O campo sob validação deve existir em um determinado banco de dados.

**Uso básico da regra**

	'state' => 'exists:states'

**Especificando um nome de coluna**

	'state' => 'exists:states,abbreviation'

<a name="rule-image"></a>
#### image

O campo sob validação deve ser uma imagem (jpeg, png, bmp, ou gif)

<a name="rule-in"></a>
#### in:_foo_,_bar_,...

O campo sob validação deve estar incluso na lista de valores dada.

<a name="rule-integer"></a>
#### integer

O campo sob validação deve ser um valor inteiro.

<a name="rule-ip"></a>
#### ip

O campo sob validação deve ser formatado como um endereço de IP.

<a name="rule-max"></a>
#### max:_value_

The field under validation must be less than a maximum _value_. Strings, numéricos, e arquivos são validados da mesma forma como a regra `size`.

<a name="rule-mimes"></a>
#### mimes:_foo_,_bar_,...

O campo sob validação deve ter um tipo MIME que corresponde a uma das extensões listadas.

**Uso básico**

	'photo' => 'mimes:jpeg,bmp,png'

<a name="rule-min"></a>
#### min:_value_

O campo sob validação deve ter um _value_ minímo. Strings, numéricos, e arquivos são validados da mesma forma como a regra `size`.

<a name="rule-not-in"></a>
#### not_in:_foo_,_bar_,...

O campo sob validação não deve estar incluso na lista de valores dada.

<a name="rule-numeric"></a>
#### numeric

O campo sob validação deve ter um valor numérico.

<a name="rule-regex"></a>
#### regex:_pattern_

O campo sob validação deve corresponder à determinada expressão regular.

**Nota:** Ao usar o padrão `regex`, poderá ser necessário especificar as regras em um array ao invés de utilizar o delimitador pipe, especialmente se a expressão regular contém um caracter pipe.

<a name="rule-required"></a>
#### required

O campo sob validação deve estar presente nos dados de entrada.

<a name="rule-required-with"></a>
#### required_with:_foo_,_bar_,...

sThe field under validation must be present _only if_ the other specified fields are present.

<a name="rule-same"></a>
#### same:_field_

O campo _field_ deve coincidir com o campo sob validação.

<a name="rule-size"></a>
#### size:_value_

The field under validation must have a size matching the given _value_. For string data, _value_ corresponds to the number of characters. For numeric data, _value_ corresponds to a given integer value. For files, _size_ corresponds to the file size in kilobytes.

<a name="rule-unique"></a>
#### unique:_table_,_column_,_except_,_idColumn_

The field under validation must be unique on a given database table. If the `column` option is not specified, the field name will be used.

**Uso básico**

	'email' => 'unique:users'

**Especificando uma coluna**

	'email' => 'unique:users,email_address'

**Forcing A Unique Rule To Ignore A Given ID**

	'email' => 'unique:users,email_address,10'

<a name="rule-url"></a>
#### url

O campo sob validação deve ser formatada como uma URL.

<a name="custom-error-messages"></a>
## Custom Error Messages

If needed, you may use custom error messages for validation instead of the defaults. There are several ways to specify custom messages.

**Passing Custom Messages Into Validator**

	$messages = array(
		'required' => 'The :attribute field is required.',
	);

	$validator = Validator::make($input, $rules, $messages);

*Note:* The `:attribute` place-holder will be replaced by the actual name of the field under validation. You may also utilize other place-holders in validation messages.

**Other Validation Place-Holders**

	$messages = array(
		'same'    => 'The :attribute and :other must match.',
		'size'    => 'The :attribute must be exactly :size.',
		'between' => 'The :attribute must be between :min - :max.',
		'in'      => 'The :attribute must be one of the following types: :values',
	);

Sometimes you may wish to specify a custom error messages only for a specific field:

**Specifying A Custom Message For A Given Attribute**

	$messages = array(
		'email.required' => 'We need to know your e-mail address!',
	);

In some cases, you may wish to specify your custom messages in a language file instead of passing them directly to the `Validator`. To do so, add your messages to `custom` array in the `app/lang/xx/validation.php` language file.

**Specifying Custom Messages In Language Files**

	'custom' => array(
		'email' => array(
			'required' => 'We need to know your e-mail address!',
		),
	),

<a name="custom-validation-rules"></a>
## Regras de validação personalizadas

Laravel provides a variety of helpful validation rules; however, you may wish to specify some of your own. One method of registering custom validation rules is using the `Validator::extend` method:

**Registering A Custom Validation Rule**

	Validator::extend('foo', function($attribute, $value, $parameters)
	{
		return $value == 'foo';
	});

> **Note:** The name of the rule passed to the `extend` method must be "snake cased".

The custom validator Closure receives three arguments: the name of the `$attribute` being validated, the `$value` of the attribute, and an array of `$parameters` passed to the rule.

Note that you will also need to define an error message for your custom rules. You can do so either using an inline custom message array or by adding an entry in the validation language file.

Instead of using Closure callbacks to extend the Validator, you may also extend the Validator class itself. To do so, write a Validator class that extends `Illuminate\Validation\Validator`. You may add validation methods to the class by prefixing them with `validate`:

**Extendendo a classe de validação**

	<?php

	class CustomValidator extends Illuminate\Validation\Validator {

		public function validateFoo($attribute, $value, $parameters)
		{
			return $value == 'foo';
		}

	}

Após, você precisar registrar sua extensão do validador personalizada:

**Registering A Custom Validator Resolver**

	Validator::resolver(function($translator, $data, $rules, $messages)
	{
		return new CustomValidator($translator, $data, $rules, $messages);
	});

When creating a custom validation rule, you may sometimes need to define custom place-holder replacements for error messages. You may do so by creating a custom Validator as described above, and adding a `replaceXXX` function to the validator.

	protected function replaceFoo($message, $attribute, $rule, $parameters)
	{
		return str_replace(':foo', $parameters[0], $message);
	}
