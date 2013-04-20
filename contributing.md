# Contribuindo para Laravel

- [Introdução](#introduction)
- [Pull Requests](#pull-requests)
- [Coding Guidelines](#coding-guidelines)

<a name="introduction"></a>
## Introdução

Laravel é gratuito, open-source, ou seja, qualquer um pode contribuir para o seu desenvolvimento e progresso. O código fonte do Laravel está hospedado no [Github](http://github.com), que fornece um método fácil para dar fork no projeto e mesclar suas contribuições.

<a name="pull-requests"></a>
## Pull Requests

The pull request process differs for new features and bugs. Before sending a pull request for a new feature, you should first create an issue with `[Proposal]` in the title. The proposal should describe the new feature, as well as implementation ideas. The proposal will then be reviewed and either approved or denied. Once a proposal is approved, a pull request may be created implementing the new feature. Pull requests which do not follow this guideline will be closed immediately.

Pull requests for bugs may be sent without creating any proposal issue. If you believe that you know of a solution for a bug that has been filed on Github, please leave a comment detailing your proposed fix.

### Pedidos de Funcionalidades

Se você tem uma idéia para uma nova funcionalidade que você gostaria de ver adicionada ao Laravel, você pode criar um issue no Github com `[Request]` no título. O pedido irá ser revisado por um colaborador do projeto.

<a name="coding-guidelines"></a>
## Coding Guidelines

Laravel segue o padrão de desenvolvimento [PSR-0](https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-0.md) e [PSR-1](https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-1-basic-coding-standard.md). Em adição a essas normas, a seguir está uma lista de outros padrões de desenvolvimentos que devem ser seguidos:

- Declarações de Namespace devem estar na mesma linha que `<?php`.
- Class opening `{` should be on the same line as the class name.
- Function and control structure opening `{` should be on a separate line.
- Sempre utilize `and` e `or`. Nunca use `&&` ou `||`.
- Nomes de Interfaces sempre com o sufixo `Interface` (`FooInterface`)
