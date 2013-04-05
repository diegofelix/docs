# Paginação

- [Configuração](#configuration)
- [Modo de usar](#usage)

<a name="configuration"></a>
## Configuração

Em outros frameworks, a paginação pode ser muito difícil. Laravel a torna fácil. Há uma única opção de configuração no arquivo `app/config/view.php`. A opção `pagination` especifica qual view deve ser usada para criar os links de paginação. Por padrão, Laravel inclui duas views.

A view `pagination::slider` irá mostrar um intervalo inteligente de links com base na página atual, enquanto a view `pagination::simple` irá simplesmente exibir os links "anterior" e "próximo". **Ambas as views são compatíveis com o Twitter Bootstrap.**

<a name="usage"></a>
## Modo de usar

Existem várias maneiras de paginar os resultados. A mais simples é usando o método `paginate` na query builder ou em um model Eloquent.

**Paginando resultados do banco de dados**

	$users = DB::table('users')->paginate(15);

Você também pode paginar os models [Eloquent](/docs/eloquent):

**Paginando um model Eloquent**

	$users = User::where('votes', '>', 100)->paginate(15);

O argumento passado para o método `paginate` é o número de itens que você deseja exibir por página. Depois de ter os resultados, você pode exibi-los em sua view, e criar os links de paginação usando o método `links`:

	<div class="container">
		<?php foreach ($users as $user): ?>
			<?php echo $user->name; ?>
		<?php endforeach; ?>
	</div>

	<?php echo $users->links(); ?>

Isso é tudo o que é preciso para criar um sistema de paginação! Observe que não precisamos informar ao framework a página atual. Laravel var determinar isso para você automáticamente.

As vezes você deseja criar os links de paginação manualmente, passando um array de itens. Você pode fazer isto usando o método `Paginator::make`:

**Criando um paginador manualmente**

	$paginator = Paginator::make($items, $totalItems, $perPage);
