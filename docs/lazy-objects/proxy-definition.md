# Proxy Definition

Proxy definition is nothing more than a object that describe which class should be wrapped, which properties of this class
should be lazy loaded and which method should be replaced.

Here we have simple exmaple:

```php
<?php

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

Read about [Initializers](initializers.md)
