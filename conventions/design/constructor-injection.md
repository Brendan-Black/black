# Pass dependencies in; don't reach for them

Code receives its collaborators from whoever creates or calls it, as constructor arguments or function parameters. It
never fetches them from a global, a static, a service locator, or the environment. Exception: genuinely optional
dependencies, which are rare. Most are a default value in disguise.

```
OrderShipper(warehouse: Warehouse, clock: Clock)     // built with its collaborators
ship(order, warehouse: Warehouse, clock: Clock)      // or handed them per call
```

- **Dependencies are visible.** The signature lists everything the code needs. Nine collaborators means it is doing
  too much, and fetching them from somewhere hides that.
- **Tests need no framework.** `OrderShipper(pseudoWarehouse, fixedClock)`.
- **Nothing is half-built.** A value can't exist without its dependencies, and they don't change afterwards
  ([restrictive defaults](restrictive-defaults.md)).

How to pass them in (constructor, generic parameter, trait object, plain function argument) is in the language's
guide.
