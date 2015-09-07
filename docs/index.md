![Isolate](/img/logo.jpg)


# Treat your data storage as an implementation detail

Almost every single web application nowadays use some kind of storage. It can be SQL Database, NoSql Database, filesystem or
some kind of key value storage like Redis. The mission of Isolate is to provide common interfaces that describe the operations
required to save data in the storage without making your application aware of used storage type.
Thanks to this simple abstraction layer application isn't bound to any storage type which means it can be easily replaced. 

How does it look like
```
$entity = $entityRepository->findEntityById(1);

$transaction = $isolate->openTransaction();
$transaction->persist($entity);

$entity->changeEntityState('new value');

$transaction->closeTransaction();
```

### Why to use Isolate (or similar abstraction layer)?

Most important and probably the only argument is a **possibility to change storage without touching 
application code**. Ok but how many times storage is replaced during application lifecycle? Once, maybe twice? 
Still such change without proper abstraction can be deadly for a project, and how many times project can actually die? 
Anyway data storage abstraction should not be used because you will change application storage, it should be used because 
you might want (or be forced) to do it someday. Think about it as about a long term investment. 

Isolating storage related code from application allows that application to be tested not only against in memory storage implementation.
Simplified filesystem based storage could be quite useful for integration tests especially for complex systems.

# Implementations

Isolate by itself does not implement any data storage, this mean that Isolate gives interfaces you can use to implement your 
data saving logic. But it does not mean you need to write those implementations every time by yourself. 

Isolate brings you following implementations.

### Isolate Framework

*Most flexible but also quite complex implementation of Isolate, it is build on top of following components:*

- [Unit of Work](https://github.com/isolate-org/unit-of-work)
- [Lazy Objects](https://github.com/isolate-org/lazy-objects) 

It was made for custom use cases where each change needs to be handled in custom way

**Perfect for:**

- webservice as a storage
- filesystem as a storage
- large systems that can't afford auto generated, not optimized Sql queries
- applications based on pure [Doctrine DBAL](http://www.doctrine-project.org/projects/dbal.html)


### Isolate Doctrine Bridge

*Easiest to use, very powerful and probably best choice for most of use cases. This bridge provides 
[Doctrine ORM and ODM](http://www.doctrine-project.org/) implementation of Isolate Transactions. 
It has no performance impact.*
 
**Perfect for:**

- startups 
- applications based on any kind of Sql or NonSql database



> *One application can use more than one Isolate implementation, some parts might be stored in database when other 
> can be stored in webservice. It's quite common in big systems, that is why you should use abstraction layer like Isolate to make 
> your code not aware of how many or what kind of storage types are used.*

# Integrations

Isolate can be integrated easily with following frameworks:

- [Symfony2](extensions/symfony/installation-and-configuration.md)
- Laravel - we are looking for some help here
- Zend2 - we are looking for some help here

We also provide extensions for following libraries:

- [Doctrine ORM and ODM](extensions/doctrine/bridge.md)
- [Tactician Command Bus](extensions/tactician/bridge.md)

# License

Isolate, libraries and this documentation are licensed under the [MIT license](https://github.com/isolate-org/documentation/blob/master/LICENSE).

# Code Versioning

Whole Isolate project follows the [semantic versioning rules](http://semver.org/) which means there will be no BC breaks
between MAJOR version changes. 
Classes, interfaces and methods that are considered as public API are marked with ``@api`` docblock.

# Contact

If you have any questions about Isolate, or need any help with the implementation or just want to chat we have an 
[twitter account @isolate_php](https://twitter.com/isolate_php).
You can also send a email to [norbert@orzechowicz.pl](mailto:norbert@orzechowicz.pl)