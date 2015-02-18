Unit of Work is a guardian of your entities states.

**What is entity you may ask?**

Actually any object that can be identified, saved in storage and loaded from using identifier is an entity for Isolate.
Each time when you create new entity it should be registered in Unit of Work. Thanks to that until object is destroyed
Unit of Work will keep original state of this object in order to build changes when you decided to save it in your storage.

**How does it works?**

In order to make Unit of Work aware of entities first you need to create [definitions](entity-definition.md) for them.


