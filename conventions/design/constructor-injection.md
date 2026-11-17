# Pass dependencies in; don't reach for them

A unit of code receives its collaborators from whoever creates or calls it: as arguments to whatever builds it, or as
parameters of the function that uses them. It never fetches them from a global, a static, a service locator, or the
environment. Exception: genuinely optional dependencies, which are rarer than they look: most turn out to be a default
value in disguise.

```
OrderShipper(warehouse: Warehouse, clock: Clock)     // built with its collaborators
ship(order, warehouse: Warehouse, clock: Clock)      // or handed them per call
```

- **Dependencies are visible.** The signature lists everything the code needs. One with nine collaborators is doing
  too much; a dependency fetched from somewhere hides that.
- **Tests need no framework.** `OrderShipper(pseudoWarehouse, fixedClock)` is the whole setup.
- **Nothing is half-built.** A value can't exist without its dependencies, and once given they don't change
  ([restrictive defaults](restrictive-defaults.md)).

How to pass them in (constructor, generic parameter, trait object, plain function argument) is the language's
business; its guide says which.
