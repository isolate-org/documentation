# Isolate

Welcome to Isolate project homepage.
Isolate is PHP framework that will help you in isolating different layers of your application.

The main goal of Isolate is to provide a [**Unit of Work**](http://martinfowler.com/eaaCatalog/unitOfWork.html) that
is not bound to any persistence layer. With Isolate you will be able to work with your code like when you
use ORM but it doesn't force you to use database as a storage for objects.

Isolate brings to you all best practices and patterns known from ORMs and it but with a possibility
to create your own behavior when objects are created, changed or removed.

Isolate is build on top of standalone independent libraries.

- [Unit of Work](unit-of-work/getting-started.md)
- [Lazy Objects](lazy-objects/getting-started.md)

Those libraries are integrated in [Isolate](https://github.com/isolate-org/isolate).
You can integrate isolate with following frameworks:

- [Symfony2](integration/symfony-bundle.md)
