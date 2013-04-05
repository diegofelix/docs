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
		'welcome' => 'Bem-vindo a sua aplicação!'
	);

O idioma padrão da sua aplicação está definido em `app/config/app.php`. Você pode alterar o idioma ativo a qualquer momento usando o método `App::setLocale`:

**Alterando o idioma ativo em tempo de execução**

	App::setLocale('es');

<a name="basic-usage"></a>
## Uso Básico

**Recuperando linhas do arquivo de idioma**

	echo Lang::get('messages.welcome');

A primeira parte da string passada para o método `get` é o nome do arquivo do idioma e o segundo é o texto a ser traduzido.

> **Nota*: Se uma tradução não existir, o método `get` retornará o valor chave original.

**Realizando substituições em um texto à ser traduzido**

Você pode definir valores a serem interpretados em uma linha de tradução:

	'welcome' => 'Bem-vindo, :name',

E então passar como um segundo argumento no método `Lang::get` o valor a ser substituído:

	echo Lang::get('messages.welcome', array('name' => 'Dayle'));

**Verificando se um arquivo de tradução possui linhas**

	if (Lang::has('messages.welcome'))
	{
		//
	}

<a name="pluralization"></a>
## Plurais

Palavras em plural são um problema complexo, pois cada idioma possui uma variedade de regras complexas para estes casos. Você pode gerenciá-los de uma maneira simples. Usando o "|" é possível separar a forma singular e plural de um texto:

	'apples' => 'Existe uma maça|Existem várias maças',

Você também pode usar o método `Lang::choice` para recuperar uma linha de tradução:

	echo Lang::choice('messages.apples', 10);

Como a tradução do Laravel é provida pelo componente Symfony Translation, você também pode criar regras de plurais explicitas facilmente:

	'apples' => '{0} Não há nenhum|[1,19] Há alguns|[20,Inf] Há muitos',
