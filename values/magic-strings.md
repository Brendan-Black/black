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

## What the build checks

Each language's tooling reports a *repeated literal*: the same string value in two literals of one project's non-test
code (a Rust package, a Gradle project's main source set), listing every place it appears. Each language implements
the same definition:

- **Identical, or not a match.** Same characters, same case, nothing trimmed or normalised. `"orderId"` matches
  `"orderId"` and nothing else: not `"orderid"`, and not `"orderIdList"`, which contains it.
- **Values, not spellings.** Escapes are resolved first, so an escaped character matches the plain one, and a raw
  string or text block matches a plain literal with the same value.

**Arguments fail the build; nothing else is reported by default.** Each literal is one of three kinds:

| Kind | What it is | A repeated value with one is |
|---|---|---|
| Argument | Passed straight to a function, method, constructor or macro: `get("status")`, `List.of("a")`, and the literal side of `"SHIPPED".equals(s)` | an error |
| Elsewhere | Assignments, returns, comparisons, `case` and match arms | not reported, if it has no argument (a project can opt in) |
| Definition | In a constant (`const`, `static`, `static final`) or an enum constant's arguments | not reported, if every one is a definition |

Arguments are where magic strings do their damage: a key, a header, a flag, passed into code that trusts it. A value
that is only ever assigned or matched is usually one place's business, so it isn't reported unless a project asks. Two
constants with the same value are two names for two concepts that happen to share a spelling (`"metadata"` as a command
and as a table name), so they aren't reported, but a constant plus a literal is: the literal should use the constant.

- **Wrapping isn't passing.** A literal that a value is built from (`Some("hard")`, `Optional.of("hard")`, a variant
  or a record) is an argument only if that value is passed on. Returned from the one function that maps an enum to
  its spelling, it is the fix this rule asks for, not a use.
- **One function, one argument: not reported.** A value whose literals are all in one function (or one other member),
  with at most one of them an argument: a parser's `startsWith("--x=")` beside its `case "--x="`. They are read together
  and change together. Two arguments in one function, such as declaring a flag and looking it up, are the typo this
  rule is for, and stay an error.
- **Close together: not reported.** A value whose literals are all near each other, however many are arguments, with
  no constant among them: by default, on one line, such as a parser's `startsWith("--author")` and `value("--author")`.
  A project sets how near with a *proximity*: a number of lines (`0`, the default, is one line), or `-1` for one file,
  `-2` for one package (a Rust directory: a module and its submodules), or `-3` for the whole module (a Rust package,
  a Java source set), which reports nothing. A constant plus a literal is never close: the literal should use the
  constant.
- **One argument among other files: a warning.** A value passed as an argument exactly once, whose other uses are all
  in other files (or in that same function), is at most a warning. It is most often another concept with the same
  spelling, such as one flag in two commands' parsers. Two or more arguments, or another use elsewhere in the
  argument's file, keep the full level.

**A limit, not a fix:** no check that matches strings can tell two concepts that share a spelling apart. `"-n"` is
`--no-checkout` to one command and a count to another, and nothing in the syntax says so. Scoping the check to a file
would hide them, and also the case that matters most: a producer in one file and its consumer in another. The two
rules above take most of that noise; a project with a lot of it can widen its proximity or lower its level.

**Not counted at all:**

- **Messages.** Prose, by its position: the format string of formatting, printing, logging and panicking calls, the
  message of an error or exception, of an `expect` or an `assert`. The tool can't read prose for embedded values, so
  name those yourself.
- **Labels.** A literal passed straight to a parameter of the project's own code that only ever goes into a message:
  `check("JSON", token)`, where `check` puts its first argument in an error and nowhere else. A parameter that is also
  compared, stored or returned, or that goes through another call first (`log(toJson(item))`: a log line can carry
  data), is a value, and so is any parameter of code outside the project.
- **No letter.** Punctuation (`", "`, `"\n"`, `"/"`) means nothing on its own, and two numbers (`"1"`, `"404"`) are
  more often a coincidence than one value used twice.
- **Whitespace.** A value with a space, a tab or a newline in it. Keys, headers, flags and statuses have none, so it
  is almost always text for people. A project can opt in to checking these.
- **Generated code.** Files under the build directory, files marked `@Generated`, and files whose opening comment has a
  line starting `@generated`.
- **Annotations and attributes**, which configure tools, and **test code**.

The rule is a convention, so a project can change both levels, for itself or for part of itself (a Rust package, a Java
package): code you are in that predates the rule [comes first](../principles/consistency.md). How is in each
language's README.
