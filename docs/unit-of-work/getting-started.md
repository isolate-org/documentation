Unit of Work is a guardian of your object states.
Each when you create new object it should be registered in Unit of Work. Thanks to that until object is destroyed
Unit of Work will keep original state of this object in order to build changes when you decided to save it in your storage.

**How does it works?**

In order to make Unit of Work aware of entities first you need to create definitions for them.

# Entity Definition

**Entity definition** describe single class, it knows how to identify instances of this class, which properties should be observed and which of them
represents associations to other entities.

Here we have a simple example:

```php
<?php

use Isolate\UnitOfWork\Entity\ClassName;
use Isolate\UnitOfWork\Entity\Definition;

$definition = new Definition(
    new ClassName("Application\Domain\Entity\User"),
    new Definition\Identity("id")
);

$definition->setObserved([
    new Definition\Property("email"),
    new Definition\Property("firstName"),
    new Definition\Property("lastName")
]);

```

Above definition tells Unit of Work that instances of class ```Application\Domain\Entity\User``` are identified
by **property path** ``id``. It also tell him to watch for any changes in ``email``, ``firstName`` and ``lastName`` properties.

Now when you know how to create an entity definition for Unit of Work you should read how to create handlers that will
be executed each time when object state is changed.

Read about [Command Handlers](command-handlers.md)
