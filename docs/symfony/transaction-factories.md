# Transaction Factories

Consider Transaction as a simple abstraction used to isolate application from infrastructure layer.
As every abstraction transactions require implementation. Symfony Bundle allows you to define custom transaction factories.

**Transaction Factory** is used by Persistence Context to create transactions each time when it needs to open new transaction.


Symfony Bundle allows you to configure default transaction factory.

```yml
# app/config/config.yml

isolate:
    # \Isolate\Symfony\IsolateBundle\Isolate\PersistenceContext\Transaction\IsolateFactory
    default_transaction_factory: isolate.transaction.factory
```

But sometimes there is a need to have different implementations of transaction. For example when some part of application
use filesystem when other parts are using database.
Symfony Bundle is prepared for such situations and it allows you to configure specific transaction factory
for specific persistence contexts.

Example:

```yml
# app/config/config.yml
services:
    isolate.test.transaction.dummy.factory:
        class: Isolate\Symfony\IsolateBundle\Tests\Functional\Isolate\PersistenceContext\Transaction\DummyTransactionFactory

isolate:
    default_transaction_factory: isolate.transaction.factory
    persistence_contexts:
        dummy:
            transaction_factory: isolate.test.transaction.dummy.factory

```

In above example persistence context with name ``dummy`` will always use ``DummyTransactionFactory`` to create new
transactions. Any other context is going to use default transaction factory which is ``isolate.transaction.factory``

```php
$transaction = $this->get('isolate')->openTransaction('dummy');

$this->assertInstanceOf(
    'Isolate\Symfony\IsolateBundle\Tests\Functional\Isolate\PersistenceContext\Transaction\DummyTransaction',
    $transaction
);
```

> Custom transactions factories need to implement
> ``Isolate\PersistenceContext\Transaction\Factory`` interface
> and they need to be registered as a service in Symfony service container.

Read about [usage examples](usage-examples.md)
