# Value Initializer

Value Initializers are objects that implements following interface:

```php
<?php

namespace Isolate\LazyObjects\Proxy\Property;

interface ValueInitializer
{
    /**
     * @param mixed $object that property holds the property.
     * @param mixed $defaultPropertyValue default of property that was set before initialization
     * @return mixed
     */
    public function initialize($object, $defaultPropertyValue);
}
```

Each LazyProperty can have only one initializer. Each initializer is called only once and value returned by initializer
is passed to target property.

# Lazy initialization

In most cases there will be no need to initialize all properties immediately after lazy object is created. Mostly because
of performance. There is no need to create object with all associated objects when we are not going to use any of those
associated objects. That's why we need to set triggers for LazyProperty. Trigger is nothing more than a method name. First
time when trigger method is execute lazy property value is going to be loaded.

Example:

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
    [
        new LazyProperty(
            new Name("items"),
            new ItemsInitializer(),
            [new Method("getItems")]
    ]
);
```

In above example ``items`` property value will be initialized only before first execution of ``getItems`` method.


# Initialization in constructor

Sometimes you want to initialize property value immediately after lazy object is created. If this is your case you
should not set any triggers for lazy property.

```php
class LazyProperty
{
    public function __construct(Name $name, ValueInitializer $valueInitializer, $triggers = [])
    {
    }
}
```
As you can see triggers are passed as a last argument of LazyProperty. Each element of $triggers array needs to be
an instance of ``Isolate\LazyObjects\Proxy\Method``.
If you will let $triggers empty property is going to be initialized in proxy constructor.

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
    [
        new LazyProperty(
            new Name("items"),
            new ItemsInitializer(),
    ]
);
```

Read about [Replacements](replacements.md)
