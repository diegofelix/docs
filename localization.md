# Localização

- [Introdução](#introduction)
- [Arquivos de Idiomas](#language-files)
- [Uso Básico](#basic-usage)
- [Pluralização](#pluralization)

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

O primeiro segmento da string passada para o método `get` é o nome do arquivo de idioma, e o segundo é o nome da linha que deve ser retornada.

> **Nota*: Se a linha solicitada não existir, a chave irá ser retornada pelo o método `get`.

**Fazendo substituições em linhas**

Você também pode definir place-holders no seu arquivo de idioma:

	'welcome' => 'Welcome, :name',

Em seguida, passe um segundo argumento de substituição para o método `Lang::get`:

	echo Lang::get('messages.welcome', array('name' => 'Dayle'));

**Determina se o arquivo de idioma contém a linha**

	if (Lang::has('messages.welcome'))
	{
		//
	}

<a name="pluralization"></a>
## Pluralização

Pluralização é um problema complexo, pois línguas diferentes têm uma variedade de regras complexas para a pluralização. Você pode gerenciar isto facilmente no seu arquivo de idioma. Usando o caracter "pipe", você pode separar as formas de singular e plural de uma string:

	'apples' => 'There is one apple|There are many apples',

Você pode utilizar o método `Lang::choice` para retornar a linha:

	echo Lang::choice('messages.apples', 10);

Como o tradutor do Laravel é construido em cima do componente Symfony Translation, você pode também criar mais regras de pluralização explícitas facilmente:

	'apples' => '{0} There are none|[1,19] There are some|[20,Inf] There are many',