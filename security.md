# Segurança

- [Configuração](#configuration)
- [Armazenando Senhas](#storing-passwords)
- [Autenticando Usuários](#authenticating-users)
- [Protegendo Rotas](#protecting-routes)
- [Lembretes de Senhas & Redefinição](#password-reminders-and-reset)
- [Criptografia](#encryption)

<a name="configuration"></a>
## Configuração

Laravel pretende fazer a implementação de autenticação muito simples. De fato, quase tudo é configurado para você. O arquivo de configuração de autenticação está localizado em `app/config/auth.php`, que contém várias opções bem documentadas para ajustar o comportamento das facilidades de autenticação.

Por padrão, Laravel inclui um model `User` no diretório `app/models` que pode ser utilizado como o driver padrão de autenticação do Eloquent. Por favor lembre-se ao construir o Schema para este Model que o campo senha tem no minímo 60 caracteres.

Se sua aplicação não está usando Eloquent, você pode usar o driver de autenticação `database` que utiliza a query builder do Laravel.

<a name="storing-passwords"></a>
## Armazenando Senhas

The Laravel `Hash` class provides secure Bcrypt hashing:

**Hashing A Password Using Bcrypt**

	$password = Hash::make('secret');

**Verifying A Password Against A Hash**

	if (Hash::check('secret', $hashedPassword))
	{
		// The passwords match...
	}

<a name="authenticating-users"></a>
## Autenticando Usuários

Para logar um usuário em sua aplicação, você pode utilizar o método `Auth::attempt`.

	if (Auth::attempt(array('email' => $email, 'password' => $password)))
	{
		// The user's credentials are valid...
	}

Take note that `email` is not a required option, it is merely used for example. You should use whatever column name corresponds to a "username" in your database.

If you would like to provide "remember me" functionality in your application, you may pass `true` as the second argument to the `attempt` method, which will keep the user authenticated indefinitely (or until they manually logout):

**Authenticating A User And "Remembering" Them**

	if (Auth::attempt(array('email' => $email, 'password' => $password), true))
	{
		// The user is being remembered...
	}

**Note:** If the `attempt` method returns `true`, the user is considered logged into the application.

**Authenticating A User With Conditions**

You may add in extra conditions to ensure that the user is (for example) 'active', or 'not suspended':

    if (Auth::attempt(array('email' => $email, 'password' => $password, 'active' => 1, 'suspended' => 0)))
    {
        // The user is active, not suspended, and exists.
    }

Uma vez que o usuário é autenticado, você pode acessar o model / registro do usuário:

**Acessando o usuário logado**

	$email = Auth::user()->email;

Para simplesmente conectar um usuário na aplicação usando o seu ID, use o método `loginUsingId`:

	Auth::loginUsingId(1);

O método `validate` permite validar as credenciais de um usuário sem realmente registrá-las na aplicação:

**Validando as credenciais de um usuário sem login**

	if (Auth::validate($credentials))
	{
		//
	}

You may also use the `stateless` method to log a user into the application for a single request. No sessions or cookies will be utilized.

**Logging A User In For A Single Request**

	if (Auth::stateless($credentials))
	{
		//
	}

**Logging A User Out Of The Application**

	Auth::logout();

<a name="protecting-routes"></a>
## Protegendo Rotas

Route filters may be used to allow only authenticated users to access a given route. Laravel provides the `auth` filter by default, and it is defined in `app/filters.php`.

**Protecting A Route**

	Route::get('profile', array('before' => 'auth', function()
	{
		// Only authenticated users may enter...
	}));

### CSRF Protection

Laravel provides an easy method of protecting your application from cross-site request forgeries.

**Insert the CSRF token into your form ** using `csrf_token()` or `Session::getToken()`

    <input type="hidden" name="_token" value="<?php echo csrf_token(); ?>">

**Validate the submitted CSRF token**

    Route::post('register', array('before' => 'csrf', function()
    {
        return 'You gave a valid CSRF token!';
    }));

<a name="password-reminders-and-reset"></a>
## Lembretes de Senhas & Redefinição

### Sending Password Reminders

Most web applications provide a way for users to reset their forgotten passwords. Rather than forcing you to re-implement this on each application, Laravel provides convenient methods for sending password reminders and performing password resets. To get started, verify that your `User` model implements the `Illuminate\Auth\RemindableInterface` contract. Of course, the `User` model included with the framework already implements this interface.

**Implementing The RemindableInterface**

	class User extends Eloquent implements RemindableInterface {

		public function getReminderEmail()
		{
			return $this->email;
		}

	}

Next, a table must be created to store the password reset tokens. To generate a migration for this table, simply execute the `auth:reminders` Artisan command:

**Generating The Reminder Table Migration**

	php artisan auth:reminders

	php artisan migrate

To send a password reminder, we can use the `Password::remind` method:

**Sending A Password Reminder**

	Route::post('password/remind', function()
	{
		$credentials = array('email' => Input::get('email'));

		return Password::remind($credentials);
	});

Note that the arguments passed to the `remind` method are similar to the `Auth::attempt` method. This method will retrieve the `User` and send them a password reset link via e-mail. The e-mail view will be passed a `token` variable which may be used to construct the link to the password reset form.

> **Note:** You may specify which view is used as the e-mail message by changing the `auth.reminder.email` configuration option. Of course, a default view is provided out of the box.

You may modify the message instance that is sent to the user by passing a Closure as the second argument to the `remind` method:

	return Password::remind($credentials, function($m)
	{
		$m->subject('Your Password Reminder');
	});

You may also have noticed that we are returning the results of the `remind` method directly from a route. By default, the `remind` method will return a `Redirect` to the current URI. If an error occurred while attempting to reset the password, an `error` variable will be flashed to the session, as well as a `reason`, which can be used to extract a language line from the `reminders` language file. So, your password reset form view could look something like this:

	@if (Session::has('error'))
		{{ trans(Session::get('reason')) }}
	@endif

	<input type="text" name="email">
	<input type="submit" value="Send Reminder">

### Resetting Passwords

Once a user has clicked on the reset link from the reminder e-mail, they should be directed to a form that includes a hidden `token` field, as well as a `password` and `password_confirmation` field. Below is an example route for the password reset form:

	Route::get('password/reset/{token}', function($token)
	{
		return View::make('auth.reset')->with('token', $token);
	});

And, a password reset form might look like this:

	@if (Session::has('error'))
		{{ trans(Session::get('reason')) }}
	@endif

	<input type="hidden" name="token" value="{{ $token }}">
	<input type="text" name="email">
	<input type="password" name="password">
	<input type="password" name="password_confirmation">

Again, notice we are using the `Session` to display any errors that may be detected by the framework while resetting passwords. Next, we can define a `POST` route to handle the reset:

	Route::post('password/reset/{token}', function()
	{
		$credentials = array('email' => Input::get('email'));

		return Password::reset($credentials, function($user, $password)
		{
			$user->password = Hash::make($password);

			$user->save();

			return Redirect::to('home');
		});
	});

If the password reset is successful, the `User` instance and the password will be passed to your Closure, allowing you to actually perform the save operation. Then, you may return a `Redirect` or any other type of response from the Closure which will be returned by the `reset` method. Note that the `reset` method automatically checks for a valid `token` in the request, valid credentials, and matching passwords.

Also, similarly to the `remind` method, if an error occurs while resetting the password, the `reset` method will return a `Redirect` to the current URI with an `error` and `reason`.

<a name="encryption"></a>
## Criptografia

Laravel provides facilities for strong AES-256 encryption via the mcrypt PHP extension:

**Encrypting A Value**

	$encrypted = Crypt::encrypt('secret');

> **Note:** Be sure to set a 32 character, random string in the `key` option of the `app/config/app.php` file. Otherwise, encrypted values will not be secure.

**Decrypting A Value**

	$decrypted = Crypt::decrypt($encryptedValue);
