# Name magic strings

**Convention.** A *magic string* is a literal that carries meaning: a status, a header name, a config key, a JSON
field, a role, an event type. If one appears more than once, name it: an enum, or a constant. Exception: used once,
handled in one place, prose, test data (below).

`status == "SHIPPED"` in one place and `status = "SHIPED"` in another compiles, runs, and is wrong forever. The
compiler checks identifiers, not strings: a typo in a literal isn't a compile error, isn't a test failure unless that
exact path was tested, and isn't a search hit when someone looks for every place that handles shipped orders. Naming
it fixes all three: typos fail to compile, "find usages" finds every use, and a rename changes them all.

## Enum or constant?

- **Closed set that travels: an enum.** Statuses, roles, event types, error codes: anything whose legal values you
  could list today, and which is stored, passed around, or switched on in more than one place. An enum makes illegal
  values unrepresentable and gets exhaustive matching ([expected failures](../errors/expected-failures.md)).
- **Open set: constants.** Header names, config keys, metric names, query parameter names: fixed strings, but not a
  set you would switch on. Group them by concept, near the code that owns the concept (`Headers`, `ConfigKeys`,
  `MetricNames`). A project-wide `Constants` becomes a junk drawer every module depends on.

## What doesn't count

- **Strings used once.** Already as findable as it gets. Name it at the second use, or earlier if that reads better.
- **Wire values handled in one place.** A protocol's action names, dispatched by one match whose default arm rejects
  the rest, can stay literals. Outside input needs that default arm anyway, so an enum adds a lookup and buys no
  exhaustiveness. `"GET /description.xml" =>` also says what it matches, which a concatenation of constants doesn't.
- **Prose.** Log messages, error messages, and UI copy are text for people, not values the code branches on, except
  anything embedded in them that *is* a value, such as an error code.
- **Test data.** `"John Smith"` in a fixture is data, not a contract. Expected values that mirror a production
  constant should reference it, though, or the test keeps passing after the production value changes.
