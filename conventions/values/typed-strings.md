# Wrap typed strings

A [typed string](../../definitions/typed-string.md) varies but has a shape: an email address, a currency code, a SKU.
Give each shape a type whose construction enforces it. Exception: free text; strings that never leave a function;
strings you built from a template (below).

`reserve(sku: String, warehouseId: String, customerId: String)` accepts `reserve(customerId, sku, warehouseId)`:
three strings are three strings. The shape lives in a regex somewhere, or three slightly different ones, or only in
heads. Every function that receives the string must re-validate it, trust that upstream did, or not think about it at
all, usually the last.

Write the shape down as a wrapper whose only constructor validates: `reserve(Sku, WarehouseId, CustomerId)`.

- **Validation happens once, at the edge.** Convert the raw string where it enters (a request, a file, a column); from
  then on, holding a `Sku` is proof it is valid ([fail early](../errors/fail-early.md)).
- **Mix-ups stop compiling.** Swapped arguments are a type error.
- **The rule has one home.** "Find usages" shows everywhere SKUs flow, and a format change is one edit.
- **Signatures explain themselves.** No doc comment needed to say what the strings are.

A wrapper's string form may be its canonical value ([debug strings](debug-strings.md)). A [marker
interface](../../definitions/marker-interface.md) (`TypedString`) on each is not an [unused
abstraction](../design/no-unused-abstractions.md): "find implementations" gives a catalogue of every shaped value in
the system.

Cost and the escape hatch: [typed-strings-cost](typed-strings-cost.md), [trusted-path](trusted-path.md).

## When not to bother

- **Free text.** Names, descriptions, comments, search queries have no shape, and inventing one (names must be letters
  only) rejects real people's real data.
- **Strings that never leave a function.** Parsed and consumed within a few lines, a type gains little.
- **Strings your code builds from a template** out of already trusted inputs. A shape check would only confirm the
  line above it. Wrap anyway if the type does something a string can't, such as building URLs or redacting itself.

Check the *shape* only. "Is this a well-formed SKU?" belongs in the constructor; "does this SKU exist?" needs a
database and belongs in a service.

**Opaque tokens** (session tokens, API keys, pagination cursors) have no shape to validate. They get a wrapper so that
nobody parses, logs, or compares them by hand. Expose nothing but what the token is for, and redact its string form
([secrets](../../correctness/values/secrets-in-logs.md)).
