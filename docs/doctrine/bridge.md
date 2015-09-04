# Doctrine Bridge

[Doctrine](http://www.doctrine-project.org/) is an awesome project which can really simplify your life.

**It's recommended to use Doctrine each time when you need to deal with any kind of database.**

However sometimes when project grows we realize that not everything fits to relational database. If there is no abstraction
that isolate your code (application layer) from doctrine (infrastructure layer) migration to webservice, non relational database
or any other storage type might be impossible. If you want to avoid such situations in future Isolate have perfect solution.
Thanks to Isolate Doctrine Bridge you can keep layers isolated and still use the power of Doctrine (ORM and ODM).

This bridge provides custom transaction factory ``Isolate\PersistenceContext\Transaction\DoctrineFactory``
which can create ``ORMTransaction`` and ``ODMTransaction``.

How ``DoctrineFactory`` knows which transaction should be created?
It's using persistence context name to get object manager from ``Doctrine\Common\Persistence\ManagerRegistry``.

- If object manager implements ``EntityManagerInterface`` then ``ORMTransaction`` is created.
- If object manager implements ``DocumentManager`` then ``ODMTransaction`` is created.

Both transactions use object manager under the hood.
However there is one limitation:

- **ODMTransaction does not allow you to rollback after transaction failure**.

You can use Doctrine Bridge directly with [Symfony Bundle](../symfony/doctrine-bridge.md)
