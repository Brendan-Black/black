# Wrap typed strings

**Convention.** *Typed strings* are values that vary but have a shape: an email address, a currency code, a SKU, a
tenant ID, a version number. Give each shape a type whose construction enforces it. Exception: free text; strings that
never leave a function; strings you built from a template (below).

`reserve(sku: String, warehouseId: String, customerId: String)` accepts `reserve(customerId, sku, warehouseId)`:
three strings are three strings. The shape exists but isn't written down. It lives in a regex somewhere, or three
slightly different ones, or only in heads, and every function that receives the string must re-validate it, trust
upstream did, or not think about it at all, usually the last. A value that was never the right shape travels deep into
the system before anything notices.

Write the shape down as a type: a wrapper whose only constructor validates, so `reserve(Sku, WarehouseId,
CustomerId)`. What that buys:

- **Validation happens once, at the edge.** Convert the raw string where it enters (a request, a file, a column); from
  then on, holding a `Sku` is proof it is valid ([fail early](../errors/fail-early.md)).
- **Mix-ups stop compiling.** Swapped arguments are a type error, not a production incident.
- **The rule has one home.** One pattern, "find usages" shows everywhere SKUs flow, and a format change is one edit.
- **Signatures explain themselves.** No doc comment needed to say what the strings are.

These are the small value wrappers whose string form may be their canonical value
([debug strings](debug-strings.md)). A marker interface (`TypedString`) on them is not an
[unused abstraction](../design/no-unused-abstractions.md): its user is the reader, and "find implementations" gives a
catalogue of every shaped value in the system.

Cost and the escape hatch: [typed-strings-cost](typed-strings-cost.md), [trusted-path](trusted-path.md).

## When not to bother

- **Free text.** Names, descriptions, comments, search queries have no shape, and inventing one (names must be letters
  only) rejects real people's real data.
- **Strings that never leave a function.** Parsed and consumed within a few lines, a type gains little.
- **Strings your code builds from a template** out of already trusted inputs: a shape check can only confirm the line
  above it. Unless the type also does something a string can't, such as building URLs or redacting itself.

Keep the check to the *shape*. "Is this a well-formed SKU?" belongs in the constructor; "does this SKU exist?" needs a
database and belongs in a service.

**Opaque tokens** (session tokens, API keys, pagination cursors) need a type *because* you must not look inside them.
No shape to validate; the wrapper makes the value impossible to mistake for a string you can parse, log, or compare by
hand. Expose nothing but what the token is for, and redact its string form ([secrets](secrets-in-logs.md)).
