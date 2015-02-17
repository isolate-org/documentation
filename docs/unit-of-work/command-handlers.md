As you already know before Unit of Work can track your object states you need to create definitions for each object class.
Each time when object is registered in Unit of Work and its changed and you decide to save it state Unit of Work create
Command and check if there is a handler for it.

There are three command types.

- ``Isolate\UnitOfWork\Command\NewCommand`` created when object was never persisted.
- ``Isolate\UnitOfWork\Command\EditCommand`` created when object was persisted but it was modified after registration in UoW.
- ``Isolate\UnitOfWork\Command\RemoveCommand`` created when object was persisted but it should be removed.

Command handlers are registered on entity definitions.

```php
<?php

use Application\Entity\Definition\User\NewCommandHandler;
use Application\Entity\Definition\User\EditCommandHandler;
use Application\Entity\Definition\User\RemoveCommandHandler;
use Isolate\UnitOfWork\Entity\ClassName;
use Isolate\UnitOfWork\Entity\Definition;

$definition = new Definition(
    new ClassName("Application\Domain\Entity\User"),
    new Definition\Identity("id")
);

$definition->setObserved([
    new Definition\Property("email"),
    new Definition\Property("birthDate")
]);

$definition->setNewCommandHandler(new NewCommandHandler());
$definition->setEditCommandHandler(new EditCommandHandler());
$definition->setRemoveCommandHandler(new RemoveCommandHandler());
```

Now each time when Unit Of Work will detect that User object that was registered should be persisted, updated or removed
during commit it will execute handle method on selected handler.

For each example in this part of readme we are going to use following User class

# User Class
```php
<?php

namespace Application\Domain\Entity;

use Doctrine\Common\Collections\ArrayCollection;
use GithubManager\Domain\Exception\RuntimeException;
use GithubManager\Domain\User\Username;

class User
{
    private $id;

    private $email;

    private $birthDate;

    public function __construct($email)
    {
        $this->email = $email;
    }

    public function setBirthDate(\DateTime $birthDate)
    {
        $this->birthDate = $birthDate;
    }
}

```

# New Command Handler

Following example explains how to create new command handler. In this example we are going to use ``Storage`` object
that will represent any kind of storage.

```php
<?php

namespace use Application\Entity\Definition\User;

use Application\Infrastructure\Storage;
use Isolate\UnitOfWork\Command\NewCommand;
use Isolate\UnitOfWork\Command\NewCommandHandler as Handler;

class NewCommandHandler implements Handler
{
    private $storage;

    public function __construct(Storage $storage)
    {
        $this->storage = $storage;
    }

    public function handle(NewCommand $command)
    {
        /* @var \Application\Domain\Entity $entity */
        $entity = $command->getEntity()
        $this->storage->saveNew($entity);
    }
}
```

In order to execute new command handler we need to register in Unit of Work instance of [User](#user-class)
that have empty ``id`` property and commit changes.

```php

$unitOfWork = $serviceContainer->get('unit_of_work');

$user = new User('norbert@orzechowicz.pl');
$unitOfWork->register($user);

$unitOfWork->commit();

```

# Edit Command Handler

Following example explains how to create edit command handler. In this example we are going to use ``Storage`` object
that will represent any kind of storage.

```php
<?php

namespace use Application\Entity\Definition\User;

use Application\Infrastructure\Storage;
use Isolate\UnitOfWork\Command\EditCommand;
use Isolate\UnitOfWork\Command\EditCommandHandler as Handler;

class EditCommandHandler implements Handler
{
    private $storage;

    public function __construct(Storage $storage)
    {
        $this->storage = $storage;
    }

    public function handle(EditCommand $command)
    {
        /* @var \Application\Domain\Entity $entity */
        /* @var \Isolate\UnitOfWork\Entity\Value\ChangeSet $changes*/
        /* @var \Isolate\UnitOfWork\Entity\Value\Change $change*/
        $entity = $command->getEntity()
        $changes = $command->getChanges();

        if ($changes->hasChangeFor("birthDate")) {
            $change = $changes->getChangeFor("birthDate");
            $this->storage->update($entity, ['birthDate' => $change->getNewValue()]);
        }
    }
}
```

In order to execute edit command handler we need to register in Unit of Work instance of [User](#user-class)
that have not empty ``id`` property, change one of observed properties and commit changes.

```php

$unitOfWork = $serviceContainer->get('unit_of_work');
$storage = $serviceContainer->get('storage');
$user = $storage->getUserByEmail('norbert@orzechowicz.pl');

$unitOfWork->register($user);

$user->setBirthDate(new \DateTime('1989-01-01'));

$unitOfWork->commit();

```

# Remove Command Handler

Removing objects is different from creating and editing because we are not going to remove instance of entity, we are just
going to tell Unit of Work that this specific entity needs to be removed.
Following example explains how to create edit command handler. In this example we are going to use ``Storage`` object
that will represent any kind of storage.

```php
<?php

namespace use Application\Entity\Definition\User;

use Application\Infrastructure\Storage;
use Isolate\UnitOfWork\Command\RemoveCommand;
use Isolate\UnitOfWork\Command\RemoveCommandHandler as Handler;

class RemoveCommandHandler implements Handler
{
    private $storage;

    public function __construct(Storage $storage)
    {
        $this->storage = $storage;
    }

    public function handle(RemoveCommand $command)
    {
        /* @var \Application\Domain\Entity $entity */
        $entity = $command->getEntity();
        $this->storage->remove($entity);
    }
}
```

In order to execute remove command handler we need to register in Unit of Work instance of [User](#user-class)
that have not empty ``id`` property, tell Unit of Work to remove it and commit changes.

```php

$unitOfWork = $serviceContainer->get('unit_of_work');
$storage = $serviceContainer->get('storage');
$user = $storage->getUserByEmail('norbert@orzechowicz.pl');

$unitOfWork->register($user);

$unitOfWork->remove($user);

$unitOfWork->commit();

```

Read about [Associations](associations.md)
