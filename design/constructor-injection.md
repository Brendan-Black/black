# Inject through constructors

**Convention.** Pass collaborators in through the constructor and keep them in fields that can't be reassigned. Not
framework field injection, and not setters. Exception: genuinely optional dependencies, which are rarer than they look:
most turn out to be a default value in disguise.

```
OrderShipper(warehouse: Warehouse, clock: Clock)
```

- **Dependencies are visible.** The constructor lists everything the type needs. A constructor with nine parameters is
  a type doing too much; field injection hides that.
- **Tests need no framework.** `OrderShipper(pseudoWarehouse, fixedClock)` is the whole setup.
- **The object is never half-built.** It can't exist without its dependencies.
