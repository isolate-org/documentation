# Method Replacement

Each replacement needs to implements following interface:

```php
<?php

namespace Isolate\LazyObjects\Proxy\Method;

interface Replacement
{
    /**
     * @param mixed $object
     * @param string $methodName
     * @param array $params
     * @return mixed
     */
    public function execute($object, $methodName, array $params = []);
}
```

Sometimes because of performance reasons there is no need to load all associated objects and operate on them.
Let's assume that we have a User object that is associated with 1000 Item objects. Now our goal is to remove one element
from this large items collection. Of course we can initialize full collection, remove one element that we are interested in
and save changes in storage but it will be slow and not effective.

That's why Lazy Objects allows you to create method replacements that are going to be executed instead of specific method.
Method replacements should be added to proxy definitions just like lazy properties.

Example:

```php

use Isolate\LazyObjects\Proxy\ClassName;
use Isolate\LazyObjects\Proxy\Definition;
use Isolate\LazyObjects\Proxy\LazyProperty;
use Isolate\LazyObjects\Proxy\Method;
use Isolate\LazyObjects\Proxy\MethodReplacement;
use Isolate\LazyObjects\Proxy\Property\Name;

return new Definition(
    new ClassName("Application\Domain\Entity\User"),
    [], // lazy properties
    [
        new MethodReplacement(new Method("removeItem"), new RemoveItemsReplacement())
    ]
);
```

Replacements are called after lazy property initialization so you can still make from ``removeItem`` method a trigger
for a ``items`` property.


