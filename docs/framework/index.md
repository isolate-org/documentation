# Isolate Framework

Isolate framework is build on top of [Unit of Work](unit-of-work/getting-started.md) and [Lazy Objects](lazy-objects/getting-started.md).

### Unit of Work

Allows to track changes in entities and save them in any kind of storage. It's very flexible but require from 
developer good knowledge about how storage used in application works. 

Read more about [Unit of Work](http://martinfowler.com/eaaCatalog/unitOfWork.html)

### Lazy Objects 

Allows to build objects partially, sometimes when entity has many dependencies it might be wise to load them 
only when those dependencies are required. 
Lets assume that we have User entity, each user can have his own followers and each user can follow another user. 
It's not hard to imagine that creating User entity who has 10k of followers would waste a lot of resources. 
Its a perfect use case for lazy objects, thanks to this component you can create entity with all required data expects 
followers and followers collection can be initialized only when you directly access it, for example by executing 
``User::getFollowers()`` method. 

Lazy objects component implements [Proxy pattern](https://en.wikipedia.org/wiki/Proxy_pattern)

---

**Isolate framework** provides implementation of [Isolate Transaction](../isolate/isolate.md) that use unit of work and 
lazy objects under the hood.