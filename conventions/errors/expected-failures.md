# Expected outcomes go in the return type

"Payment declined" and "username taken" are ordinary outcomes callers must handle. Put them in the return type as a
closed set (a sealed type, a sum type, a result enum), where an exhaustive match makes callers handle them. **Bugs and
infrastructure failures are exceptions** (or panics). Exception: model only the outcomes callers branch on.

```
ChargeResult = Charged(paymentId) | Declined(reason)
match payments.charge(order) { Charged(id) => confirm(order, id), Declined(r) => notifyCustomer(order, r) }
```

A new outcome is then a compile error at every call site.

`reason` is a string here because it is only passed along to the customer. Once a caller branches on it (retry on
"insufficient funds", flag on "suspected fraud"), it becomes an enum ([magic strings](../values/magic-strings.md)).

Don't overdo it. Wrapping every call in a result type buries the success path, and an error hierarchy nobody catches
is an [unused abstraction](../design/no-unused-abstractions.md).
