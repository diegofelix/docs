# Localização

- [Introdução](#introduction)
- [Arquivos de Idiomas](#language-files)
- [Uso Básico](#basic-usage)
- [Plurais](#pluralization)

<a name="introduction"></a>
## Introdução

A classe `Lang` provê uma forma conveniente de recuperar textos em vários idiomas, permitindo sua aplicação suportar vários idiomas de maneira muito simples.

<a name="language-files"></a>
## Arquivos de Idiomas

Os textos de cada idioma estão armazenados nos arquivos localizados no diretório `app/lang`. Este diretório deve conter um diretório para cada idioma suportado pela sua aplicação.

	/app
		/lang
			/en
				messages.php
			/es
				messages.php

Os arquivos de idioma simplesmente retornam um vetor com o texto traduzido associado a uma chave. Exemplo:

**Exemplo de um arquivo de idioma**

	<?php

	return array(
		'welcome' => 'Welcome to our application'
	);

O idioma padrão da sua aplicação está definido em `app/config/app.php`. Você pode alterar o idioma ativo a qualquer momento usando o método `App::setLocale`:

**Alterando o idioma ativo em tempo de execução**

	App::setLocale('es');

<a name="basic-usage"></a>
## Uso Básico

**Recuperando linhas do arquivo de idioma**

	echo Lang::get('messages.welcome');

The first segment of the string passed to the `get` method is the name of the language file, and the second is the name of the line that should be retrieved.

> **Note*: If a language line does not exist, the key will be returned by the `get` method.

**Making Replacements In Lines**

You may also define place-holders in your language lines:

	'welcome' => 'Welcome, :name',

Then, pass a second argument of replacements to the `Lang::get` method:

	echo Lang::get('messages.welcome', array('name' => 'Dayle'));

**Determine If A Language File Contains A Line**

	if (Lang::has('messages.welcome'))
	{
		//
	}

<a name="pluralization"></a>
## Pluralization

Pluralization is a complex problem, as different languages have a variety of complex rules for pluralization. You may easily manage this in your language files. By using a "pipe" character, you may separate the singular and plural forms of a string:

	'apples' => 'There is one apple|There are many apples',

You may then use the `Lang::choice` method to retrieve the line:

	echo Lang::choice('messages.apples', 10);

Since the Laravel translator is powered by the Symfony Translation component, you may also create more explicit pluralization rules easily:

	'apples' => '{0} There are none|[1,19] There are some|[20,Inf] There are many',
