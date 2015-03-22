# Isolate

Isolate brings to your application persistence contexts which are responsible for opening and closing transactions.
In php our application lifetime is limited, it starts with request and ends with response.
Because of that every single entity needs to be stored somewhere. But code that represents our
business logic should not be aware of such details. Business logic related code should be able to run just
like in memory environment (without any states).

This is why Isolate gives you ability to put entities into transactions which are responsible for saving
states of those entities in storage when they are closed.

Thanks to Isolate Unit of Work is hidden under transaction which can be changed at any point of your application lifecycle.
Transaction is an missing abstraction between your application and any kind of data storage. Thanks to this simple interface
you can easily start with database as a storage and switch to the web services without changing anything in your application
layer. It would be as simple as replacing entity change handlers registered in entity definitions. Isn't that great?

## Isolate Core Concepts

## Isolate

Can be considered as registry of persistence contexts. Each context is created inside of Isolate instance
and it will remain there up to the end of this object lifecycle.
Available methods:

```php
Isolate::DEFAULT_CONTEXT = 'isolate';

Isolate::getContext($name = self::DEFAULT_CONTEXT)
```

New context is created when your first time execute ``Isolate::getContext('name')`` method. If your need to access
same context many times just be sure you are using always the same name (context names are case sensitive).

### Persistence Context**

Persistence context is responsible for opening and closing transactions. At the moment only one transaction can be open.
New transaction can be open only when old is closed.

Available methods:

```php
Isolate::getName();

Isolate::openTransaction();

Isolate::hasOpenTransaction();

Isolate::getTransaction();

Isolate::closeTransaction();
```

### Transaction

Represents all actions required to meet business requirements. IsolateTransaction is nothing more than a wrapper for
Unit of Work but thanks to such this abstraction it's possible to create DoctrineTransaction which can use
Doctrine registry under the hood.

Available methods:

```php
Isolate::contains($entity);

Isolate::persist($entity);

Isolate::delete($entity);

Isolate::commit();

Isolate::rollback();
```

# Visualization

You can imagine Isolate just like in following picture

![Isolate](img/isolate.png)
