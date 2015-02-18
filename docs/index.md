# Welcome to Isolate

Isolate is a PHP framework that will help you in isolating different layers of your application.

The main goal of Isolate is to provide a [**Unit of Work**](http://martinfowler.com/eaaCatalog/unitOfWork.html) that
is not bound to any kind of persistence layer. With Isolate you will be able to work with your code like when you
use ORM but it doesn't force you to use database as a storage for objects.

Isolate brings to you all best practices and patterns known from ORMs but with a possibility
to create your own behavior when entities are created, changed or removed.

# Building Blocks

Isolate is build on top of standalone independent libraries.

- [Unit of Work](unit-of-work/getting-started.md)
- [Lazy Objects](lazy-objects/getting-started.md)

Those libraries are integrated in [Isolate](https://github.com/isolate-org/isolate).
You can integrate isolate with following frameworks:

- [Symfony2](integration/symfony-bundle.md)

# License

Isolate, libraries and this documentation are licensed under the [MIT license](https://github.com/isolate-org/documentation/blob/master/LICENSE).
