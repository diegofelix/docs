# Ciclo de vida de Requisições

- [Visão Geral](#overview)
- [Arquivos de Inicialização](#start-files)
- [Eventos da Aplicação](#application-events)

<a name="overview"></a>
## Visão Geral

O ciclo de vida de requisições do Laravel é bem simples. Uma requisição chega a aplicação e é dispachada para a rota ou controlador apropriado. A resposta da rota é então enviada de volta para o navegador e exibida na tela. Pode ser que em algum momento, você deseje fazer algum processamento após ou antes das suas rotas serem efetivamente chamadas. Existem várias oportunidades de fazer isso, duas delas são os arquivos de inicialização ("start") e os eventos da aplicação.

<a name="start-files"></a>
## Arquivos de Inicialização

Os arquivos de inicialização de sua aplicação são armazenados em `app/start`. Por padrão, são incluidos com sua aplicação, os arquivos: `global.php`, `local.php`, e `artisan.php`. Para mais informacões sobre o arquivo `artisan.php`, veja a sessão de [Comandos do Artisan](/docs/commands#registering-commands).

O arquivo de inicialização `global.php` contém poucos itens por padrão, como o registro do Logger [Logger](/docs/errors) e a inclusão do arquivo `app/filters.php`. However, you are free to add anything to this file that you wish. Eles serão automaticamente incluídos em _cada_ requisição a sua aplicação, independentemente do ambiente. O arquivo `local.php`, por outro lado, só é chamado quando a aplicacão está executando no ambiente `local`. Para mais informacões sobre ambientes, veja a sessão [configuração](/docs/configuration) da documentação.

Obviamente, se você tem outros ambientes que não sejam o `local`, você pode criar arquivos de inicialização para esses ambientes também. Eles serão automaticamente inclusos quando a sua aplicação rodar no ambiente especificado.

<a name="application-events"></a>
## Eventos da Aplicação

Você também pode fazer processamento antes e depois das requisições registrando os eventos `before`, `after`, `close`, `finish`, e `shutdown` da aplicação:

**Registrando eventos da aplicação**

	App::before(function()
	{
		//
	});

	App::after(function($request, $response)
	{
		//
	});

Os Listeners desses eventos irão rodar antes (`before`) e depois (`after`) de cada requisição a sua aplicação.
