Isolate Lazy object is nothing more than simple proxy pattern implementation with possibility to load property value
when specific method is called or to replace behavior of selected methods.

Lazy objects should be used each time when there is no need to load full object or when data parts required to build that
object are available under different data sources. For example when data are splitted between different endpoints in
web service.

**How does it works?**

Lazy objects are created by Wrapper. Objects are wrapped in automatically generated class that extends the wrapped object
class. Each public methods of wrapped objects are overwritten in those auto generated classes but result of method is taken
from wrapped object.

How Wrapper knows which properties should be lazy loaded or which method should be replaced?
Its simple, first we need to provide him proxy definitions.
Proxy definition is nothing more than a object that describe which class should be wrapped, which properties of this class
should be lazy loaded and which method should be replaced.

Here we have simple exmaple:

```php

use Isolate\LazyObjects\Proxy\ClassName;
use Isolate\LazyObjects\Proxy\Definition;
use Isolate\LazyObjects\Proxy\LazyProperty;
use Isolate\LazyObjects\Proxy\Method;
use Isolate\LazyObjects\Proxy\MethodReplacement;
use Isolate\LazyObjects\Proxy\Property\Name;

return new Definition(
    new ClassName("Application\Domain\Entity\User"),
    [
        new LazyProperty(
            new Name("items"),
            new ItemsInitializer(),
            [new Method('getItems')]
        )
    ],
    [
        new MethodReplacement(new Method("removeItem"), new RemoveItemReplacement())
    ]
);
```

Above definition will tell Wrapper that object which implements ```Application\Domain\Entity\User``` can be wrapped.
Il will also tell him that property ``items`` of that class should be lazy loaded when method ``getItems`` is executed first time.
Value for ``items`` property should be initialized by ``ItemsInitializer`` instance.
Also method ``removeItem`` should not be called on wrapped object and it should be replaced by ``RemoveItemReplacement`` instance.

Generated proxy is going to implement ``Isolate\LazyObjects\WrappedObject`` interface and it will extend ``Application\Domain\Entity\User``
so our application will threat it as normal instance of ``Application\Domain\Entity\User``.
