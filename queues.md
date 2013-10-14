# Queues

- [Configuração](#configuration)
- [Uso Básico](#basic-usage)
- [Rodando o Listener de Queues](#running-the-queue-listener)

<a name="configuration"></a>
## Configuração

O componente de Queue do Laravel provê uma API unificada para uma variedade de serviçoes de queue diferentes. Queues permitem você adiar o processamento de uma tarefa que consome tempo, como enviar um e-mail, para outra hora, permitindo melhorar drasticamente o desenpenho das requisições em sua aplicação.

O arquivo de configuração das Queues pode ser encontrado em `app/config/queue.php`. Nesse arquivo você encontrará configurações de conexão para cada driver de queues que é incluido com o Framework, que são: [Beanstalkd](http://kr.github.com/beanstalkd), [IronMQ](http://iron.io), [Amazon SQS](http://aws.amazon.com/sqs), e synchronous (para uso local).

<a name="basic-usage"></a>
## Uso Básico

Para enviar um novo trabalho para a queue, use o método `Queue::push`:

**Enviando um trabalho para a Queue**

	Queue::push('SendEmail', array('message' => $message));

O primeirop argumento dado ao método `push` é o nome da classe que deve ser usada para processa o trabalho. O segundo argumento é um array de dados que devem ser passados ao `Handler`. Um Handler de trabalhos deve ser definido como a seguir:

**Definindo um Handler e trabalhos**

	class SendEmail {

		public function fire($job, $data)
		{
			//
		}

	}

Note que o único método obrigatório é o método `fire`, que recebe uma instância de `Job` assim como o array de dados `data` que deve ser inserido na 	queue.

Uma vêz que o trabalho foi processado, ele deve ser deletado da queue, o que pode ser feito com o método `delete` na instância `Job`:

**Deletando uma tarefa processada**

	public function fire($job, $data)
	{
		// Processa a tarefa...

		$job->delete();
	}

Se desejar mandar uma tarefa devolta para a queue, você pode utilizar o método `release`:

**Enviando um método de volta a queue**

	public function fire($job, $data)
	{
		// Processa a tarefa...

		$job->release();
	}

Você também pode especificar o número de segundos que a tarefa deve aguardar antes de voltar a queue:

	$job->release(5);

Se um Exceção ocorrer enquanto um trabalho está sendo processado, ele será automaticamente enviado de volta a queue. Você pode verificar o número de tentativas que foram feiras usando o método `attempts`:

**Verificando o número de tentativas**

	if ($job->attempts() > 3)
	{
		//
	}

<a name="running-the-queue-listener"></a>
## Rodando o Listener de Queues

O Laravel inclui uma tarefa do Artisan que irá rodar novas tarefas assim que enviadas a queue. você pode executálo atravéz do comando `queue:listen`:

**Iniciando o Listener de Queues**

	php artisan queue:listen

Você também pode especificar qual conexão o Listener de Queues deve utilizar:

	php artisan queue:listen connection

Note que uma vez que a tarefa é iniciada, ela irá permanecer rodando até que seja parada manualmente. Você talvez queira ver um monitor de processo como o  [Supervisor](http://supervisord.org/) Para garantir que o Listener não para de rodar.

Você também pode especificar a quantidade de tempo (em segundos) que cada trabalho é permitido a utilizar:

**Especificando o timeout para uma tarefa**

	php artisan queue:listen --timeout=60

Para processar apenas o primeiro trabalho da Queue, você poderá utilizar o comando `queue:work`:

**Processando o primeiro comando da Queue**

	php artisan queue:work
