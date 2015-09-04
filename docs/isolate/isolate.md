# Isolate

Isolate brings to your application persistence contexts which are responsible for opening and closing transactions.
In php our application lifetime is limited, it starts with request and ends with response.
Because of that every single entity needs to be stored somewhere. But code that represents our
business logic should not be aware of such details. Business logic related code should be able to run just
like in memory environment (without any states).

This is why Isolate gives you ability to put entities into transactions which are responsible for saving
states of those entities in storage when they are closed.

## Isolate Core Concepts

### Isolate

Can be considered as registry of persistence contexts. Each context is created inside of Isolate instance
and it will remain there up to the end of this object lifecycle.
Available methods:

```php
Isolate::DEFAULT_CONTEXT = 'isolate';

Isolate::getContext($name = self::DEFAULT_CONTEXT)
```

New context is created when your first time execute ``Isolate::getContext('name')`` method. If your need to access
same context many times just be sure you are using always the same name (context names are case sensitive).

### Persistence Context

Persistence context is responsible for opening and closing transactions. At the moment only one transaction can be open.
New transaction can be open only when old is closed.

Available methods:

```php
PersistenceContext::getName();

PersistenceContext::openTransaction();

PersistenceContext::hasOpenTransaction();

PersistenceContext::getTransaction();

PersistenceContext::closeTransaction();
```

### Transaction

It's an guardian of entities. Every single entity persisted in transaction should be watched and when transaction
is closed changes should be saved in the storage. 

Available methods:

```php
Transaction::contains($entity);

Transaction::persist($entity);

Transaction::delete($entity);

Transaction::commit();

Transaction::rollback();
```

# Visualization

You can imagine Isolate just like in following picture

![Isolate](/img/isolate.png)
