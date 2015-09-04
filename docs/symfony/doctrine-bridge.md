# Doctrine

Thanks to [Isolate Doctrine Bridge](../doctrine/bridge.md) you can use [Doctrine](http://www.doctrine-project.org/). 
In order to do so, you need to install and configure Doctrine bridge first.

```json
{
    require: {
        "doctrine/orm": "~2.2,>=2.2.3",
        "doctrine/doctrine-bundle": "~1.2",

        "isolate/doctrine-bridge": "1.0.*@dev"
    }
}
```

After installation new service ``isolate.doctrine.transaction.factory`` is available in your application.
Now everything you need to do is configure which persistence context is going to use Doctrine factory to create
transactions.

> Because DoctrineBundle register default entity manager in manager registry under name ``default``
> you should configure doctrine bundle to use a bit more unique name. In other case you will be forced to
> configure ``default`` persistence context.

```yaml
# Doctrine Configuration
doctrine:
    dbal:
        driver:   "%database_driver%"
        host:     "%database_host%"
        port:     "%database_port%"
        dbname:   "%database_name%"
        user:     "%database_user%"
        password: "%database_password%"
        charset:  "UTF8"

    orm:
        auto_generate_proxy_classes: "%kernel.debug%"
        entity_managers:
            doctrine_orm:
                auto_mapping: true
                connection: default

isolate:
    default_transaction_factory: isolate.transaction.factory
    persistence_contexts:
        doctrine_orm:
            transaction_factory: isolate.doctrine.transaction.factory
```

Usage example:

```php
<?php

class MyController extends Controller
{
    public function indexAction(Request $request)
    {
        /* @var \Isolate\PersistenceContext\Transaction\Doctrine\ORMTransaction $transaction */
        $transaction = $this->get('isolate')->openTransaction('doctrine_orm');
        $entity = new Entity("norbert@orzechowicz.pl");
        $transaction->persist($entity);
        $this->get('isolate')->closeTransaction('doctrine_orm');
    }

}
```
