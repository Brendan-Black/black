# Name magic strings

If a [magic string](../../definitions/magic-string.md) (a literal that carries meaning: a status, a header name, a
config key) appears more than once, name it: an enum, or a constant. Exception: used once, handled in one place, prose,
test data (below).

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

## Two concepts, one spelling

Name each concept, even when the values match: `SAY_TEXT_FILE` and `ESPEAK_TEXT_FILE` are both `"-f"`. Don't merge
them into one name to save a line or quiet the build: one constant for two concepts is a worse bug than the literal
was.

## What the build checks

The build fails on a string value repeated in a project's non-test code where a literal of it is **passed as an
argument** (`get("status")`), or where a literal sits beside a constant that already names it. Values only assigned,
returned, or matched are not reported by default; nor are messages, labels, values with no letter or with whitespace,
generated code, and literals that sit together in one statement. The full definition, which each language implements,
is in [magic-strings-check](magic-strings-check.md).

The settings (levels, whitespace, proximity) can change for a project or any part of it, down to a single module or
package: code that predates the rule [comes first](../../philosophy/consistency.md). How is in each language's
README.
