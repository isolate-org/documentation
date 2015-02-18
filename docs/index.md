# Welcome to Isolate

Isolate is a PHP framework that will help you in isolating different layers of your application.

The main goal of Isolate is to provide a [**Unit of Work**](http://martinfowler.com/eaaCatalog/unitOfWork.html) that
is not bound to any kind of persistence layer. With Isolate you will be able to work with your code like when you
use ORM but it doesn't force you to use database as a storage for objects.

Isolate brings to you all best practices and patterns known from ORMs but with a possibility
to create your own behavior when entities are created, changed or removed.

# Why Isolate?

To keep your application layers as clean as possible. Thanks to Unit of Work you can keep logic related to creating, updating
and deleting objects in one place. Why this is important? Because you can for example change your data storage from
database to webservice without breaking whole system.
Isolate also doesn't try to think for you. It will help you by showing how object was changed after it was registered
but it's your job to save it in data storage. Isolate will not generate any SQL queries or http calls, it will just help you to build
and execute them in proper time.

# Getting started

First you should read about [Entity Definitions](unit-of-work/getting-started.md#entity-definition). You also should
read something about [Lazy Objects](lazy-objects/getting-started.md) and after that you only need to integrate Isolate
with your system. If you are using Symfony2 Framework than problem is solved out of the box thanks to
[Isolate Symfony Bundle](integration/symfony-bundle.md)

# Building Blocks

Isolate is build on top of standalone independent libraries.

- [Unit of Work](unit-of-work/getting-started.md)
- [Lazy Objects](lazy-objects/getting-started.md)

Those libraries are integrated in [Isolate](https://github.com/isolate-org/isolate).
You can integrate isolate with following frameworks:

- [Symfony2](integration/symfony-bundle.md)

# License

Isolate, libraries and this documentation are licensed under the [MIT license](https://github.com/isolate-org/documentation/blob/master/LICENSE).
