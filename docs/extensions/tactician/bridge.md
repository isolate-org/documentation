# Tactician Bridge

Main purpose of this bridge is opening and closing transactions in middleware.

Following example is based on [Symfony2](https://github.com/symfony/symfony-standard) and [Tactician Bundle](https://github.com/thephpleague/tactician-bundle)

Configuration:

```
# app/config/services.yml

services:
    tactician.middleware.isolate.transaction:
        class: Isolate\Tactician\TransactionMiddleware
        arguments: 
            - @isolate
            # - "isolate" this is default context name
```

```
# app/config/config.yml

tactician:
    commandbus:
        default:
            middleware:
                - tactician.middleware.isolate.transaction
                - tactician.middleware.command_handler
```

This is almost everything, sometimes for example when using [Doctrine Bridge](../doctrine/bridge.md) it might 
be required to pass second parameter in order to change name of persistence context created in tactician transaction middleware. 