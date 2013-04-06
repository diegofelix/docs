# Uso básico do banco de dados

- [Configuração](#configuration)
- [Executando Queries](#running-queries)
- [Conexões de Acesso](#accessing-connections)

<a name="configuration"></a>
## Configuração

Laravel torna a conexão com o banco de dados e execução de queries extremamente simples. O arquivo de configuração do banco de dados é `app/config/database.php`. Neste arquivo você pode definir todas suas conexões com o banco de dados, bem como especificar qual conexão deve ser usada como padrão. Exemplos para todos os sistemas de bancos de dados suportados estão neste arquivo.

Atualmente Laravel suporta quatro tipos de sistemas de bancos de dados: MySQL, Postgres, SQLite, e SQL Server.

<a name="running-queries"></a>
## Executando Queries

Depois de ter configurado a conexão com o banco de dados, você pode executar queries usando a classe `DB`.

**Executando uma Query de Select**

	$results = DB::select('select * from users where id = ?', array(1));

O método `select` irá sempre retornar um `array` de resultados.

**Executando uma instrução de Insert**

	DB::insert('insert into users (id, name) values (?, ?)', array(1, 'Dayle'));

**Executando uma instrução de Update**

	DB::update('update users set votes = 100 where name = ?', array('John'));

**Executando uma instrução de Delete**

	DB::delete('delete from users');

> **Nota:** As instruções de `update` e `delete` retornam o número de linhas afetadas pela operação.

**Executando uma instrução geral**

	DB::statement('drop table users');

Você pode "escutar" por eventos de query usando o método `DB::listen`:

**"Escutando" eventos de query**

	DB::listen(function($sql, $bindings, $time)
	{
		//
	});

<a name="accessing-connections"></a>
## Conexões de Acesso

Quando utilizar multiplas conexões, você pode acessá-las através do método `DB::connection`:

	$users = DB::connection('foo')->select(...);