# Getting started

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
Its simple, first we need to provide him [proxy definitions](proxy-definition.md).
