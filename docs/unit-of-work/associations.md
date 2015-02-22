# Associations

In real world we often create associations between entities. For example lets think about order that is associated with
many items or about order that is associated customer. How to deal with such cases using Unit of Work?

First you need to add to entity property definition information about Association.

Example:

```php
<?php

use Isolate\UnitOfWork\Entity\ClassName;
use Isolate\UnitOfWork\Entity\Definition;

$definition = new Definition(
    new ClassName("Application\Domain\Entity\Order"),
    new Definition\Identity("id")
);

$definition->setObserved([
    new Definition\Property(
        "items",
        new Definition\Association(new ClassName("Application\Domain\Entity\Order\Item"), Definition\Association::TO_MANY_ENTITIES)
    ),
    new Definition\Property(
        "customer",
        new Definition\Association(new ClassName("Application\Domain\Entity\Customer"), Definition\Association::TO_SINGLE_ENTITY)
    )
]);

```

As you can see there are two types of associations that you can set.

- ``Definition\Association::TO_MANY_ENTITIES`` - when entity is associated with collection of entities
- ``Definition\Association::TO_SINGLE_ENTITY`` - when entity is associated only with one entity

Associations are only relevant in the scope of EditCommand handlers. Property value change generated for TO_MANY_ENTITIES is
an instance of ``Isolate\UnitOfWork\Entity\Value\Change\AssociatedCollection`` and change generated for TO_SINGLE_ENTITY
association can be an instance of ``Isolate\UnitOfWork\Entity\Value\Change\NewEntity``, ``Isolate\UnitOfWork\Entity\Value\Change\RemovedEntity``
or ``Isolate\UnitOfWork\Entity\Value\Change\EditedEntity``

Those different change types are going to help in building SQL queries or executing API calls in order to save entity in storage.

Simple Example how to deal with AssociatedCollection change

```php
<?php

namespace GithubManager\Application\AppBundle\Isolate\Entity\Definition\User;

use Application\Infrastructure\Storage\Client;
use Isolate\UnitOfWork\Command\EditCommand;
use Isolate\UnitOfWork\Command\EditCommandHandler as Handler;
use Isolate\UnitOfWork\Entity\Value\Change\AssociatedCollection;

class EditCommandHandler implements Handler
{
    /**
     * @var Client
     */
    private $client;

    /**
     * @param Client $client
     */
    public function __construct(Client $client)
    {
        $this->client = $client;
    }

    /**
     * @param EditCommand $command
     */
    public function handle(EditCommand $command)
    {
        $changes = $command->getChanges();
        if ($changes->hasChangeFor("followedUsers")) {
            $this->handleFollowedUsersChange($changes->getChangeFor("followedUsers"));
        }
    }

    /**
     * @param AssociatedCollection $change
     */
    private function handleFollowedUsersChange(AssociatedCollection $change)
    {
        foreach ($change->getChangesForRemovedEntities() as $removedEntityChange) {
            $this->client->unfollow($removedEntityChange->getOriginValue());
        }
    }
}

```
