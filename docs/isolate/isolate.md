# Isolate

Isolate brings to your application persistence contexts which are responsible for opening and closing transactions.
In php our application lifetime is limited, it starts with request and ends with response.
Because of that every single entity needs to be saved somewhere.

This is why Isolate introduce interface called ``Transaction``, this interface represents more or less lifetime of entities. 
Any entity can be putted into transaction and it's gonna be saved when transaction is committed. 

## Isolate Core Concepts

### Isolate

Can be considered as registry of persistence contexts. Each context is created inside of Isolate instance
and it will remain there up to the end of this object lifecycle.
Available methods:

```php
Isolate::DEFAULT_CONTEXT = 'isolate';

Isolate::getContext($name = self::DEFAULT_CONTEXT);

Isolate::openTransaction($name = self::DEFAULT_CONTEXT);

Isolate::hasOpenTransaction($name = self::DEFAULT_CONTEXT);

Isolate::getTransaction($name = self::DEFAULT_CONTEXT);

Isolate::closeTransaction($name = self::DEFAULT_CONTEXT);
```

New context is created when your first time execute ``Isolate::getContext('name')`` method. If your need to access
same context many times just be sure you are using always the same name (context names are case sensitive).

### Persistence Context

Persistence context is responsible for opening and closing transactions. It can be accessed via 
``Isolate::getContext($name = self::DEFAULT_CONTEXT)`` but in most cases it is enough to use methods available directly
on ``Isolate`` instance. 

Available methods:

```php
PersistenceContext::getName();

PersistenceContext::openTransaction();

PersistenceContext::hasOpenTransaction();

PersistenceContext::getTransaction();

PersistenceContext::closeTransaction();
```

### Transaction

Represents entities lifecycle. When new entity is created or recreated (from stored somewhere data) it should be 
persisted into transaction. When it is time to store changes transaction should be committed by ``Transaction::commit()``
During commit all changes made on entities persisted in transaction should be saved. Also if any entity was removed by
``Transaction::delete($entity)`` it should be removed from the storage during commit. 

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
