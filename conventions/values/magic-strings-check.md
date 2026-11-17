# The repeated-literal check

**Reference, not a rule.** The definition each language's tooling implements for [magic strings](magic-strings.md):
it reports a *repeated literal*, the same string value in two literals of one project's non-test code, and lists every
place it appears. What "project", "constant" and "module" mean in a given language, and the settings, are in that
language's `guide/values/magic-strings.md` and README.

## Matching

- **Identical, or not a match.** Same characters, same case, nothing trimmed or normalised. `"orderId"` matches
  `"orderId"` and nothing else: not `"orderid"`, and not `"orderIdList"`, which contains it.
- **Values, not spellings.** Escapes are resolved first, so an escaped character matches the plain one, and a raw or
  multi-line string matches a plain literal with the same value.

## Levels

**Arguments fail the build; nothing else is reported by default.** Each literal is one of three kinds:

| Kind | What it is | A repeated value with one is |
|---|---|---|
| Argument | Passed straight to a function, method, constructor or macro: `get("status")`, `listOf("a")`, and the literal receiving a method call, as in `"SHIPPED".equals(s)` | an error |
| Elsewhere | Assignments, returns, comparisons, `case` and match arms | not reported, if it has no argument (a project can opt in) |
| Definition | The value of a named constant, or an argument of an enum constant | not reported, if every one is a definition |

Arguments are where magic strings do their damage: a key, a header, a flag, passed into code that trusts it. A value
only ever assigned or matched is usually one place's business. Two constants with the same value are two names for
two concepts that share a spelling (`"metadata"` as a command and as a table name), so they aren't reported; a constant
plus a literal is, because the literal should use the constant.

These adjust the level:

- **Wrapping isn't passing.** A literal a value is built from (an optional's `some("hard")`, a variant, a record) is
  an argument only if that value is passed on. Returned from the one function that maps an enum to its spelling, it is
  the fix, not a use.
- **One function, one argument: not reported.** All of a value's literals in one function (or one other member), at
  most one of them an argument: a parser's `startsWith("--x=")` beside its `case "--x="`. They are read and changed
  together. Two arguments in one function, such as declaring a flag and looking it up, are the typo this rule is for,
  and stay an error.
- **Close together: not reported.** All of a value's literals near each other, however many are arguments, with no
  constant among them. By default, *near* is one statement, such as `case "--author" => value("--author")`. A
  branch's head (a match arm's pattern and guard, a `case`'s labels, an `if`'s condition) counts as one statement with
  the first statement it leads to, and no more: a flag declared and then looked up in one branch is two. Measured by
  the code, not its lines, so where the formatter breaks them doesn't matter. A project can widen the *proximity* to a
  function, a file, a module, or the whole project, which reports nothing.
- **One argument among other files: a warning.** A value passed as an argument exactly once, whose other uses are all
  in other files (or in that same function). It is most often another concept with the same spelling, such as one
  flag in two commands' parsers. Two or more arguments, or another use in the argument's file, keep the full level.
- **A literal beside its constant: always the full level.** When a constant holds the value and a literal of it is
  passed as an argument where that constant can be named (it is public, or the literal is in its module, or in its
  own file for a private one), nothing above softens it: the name exists, so the literal is wrong. Only a constant
  whose whole value is the string counts (an entry in a table is no name for it), and only one the literal could name:
  another module's private constant, or an executable's constant seen from its library, plays no part.
- **The same comparison in three functions: a warning.** A value never passed as an argument but compared or matched
  in three or more functions is the same check written again each time, such as four parsers of
  `"true" | "yes" | "on"`. Write the check once.

Errors are always listed. Warnings are counted on one line, with every place on request, so the build's output stays
readable.

## Not counted at all

- **Messages.** Prose, by position: the format string of formatting, printing, logging and panicking calls; the
  message of an error, an exception, or an assertion; the field labels of hand-written debug output
  ([debug strings](debug-strings.md)). The tool can't read prose for embedded values, so name those yourself.
- **Labels.** A literal passed straight to a parameter of the project's own code that only ever goes into a message:
  `check("JSON", token)`, where `check` puts its first argument in an error and nowhere else. A parameter that is also
  compared, stored or returned, or that goes through another call first (`log(toJson(item))`: a log line can carry
  data), is a value, and so is any parameter of code outside the project. Only a call that can reach the project's own
  code counts: unqualified, on `self` or `this`, or through the project's own type or module. A call into the standard
  library, such as reading a file or adding to a list, is never the project's, whatever it calls its own functions.
- **No letter.** Punctuation (`", "`, `"\n"`, `"/"`) means nothing on its own, and two numbers (`"1"`, `"404"`) are
  more often coincidence than one value. A backslash and the letter after it (`"\\t"`, an escape spelled out, as in a
  regular expression) don't count as a letter.
- **Whitespace.** A value with a space, tab or newline. Keys, headers, flags and statuses have none, so it is almost
  always text for people. A project can opt in.
- **Generated code.** Files under the build directory, and files the language's convention marks as generated.
- **Annotations and attributes**, which configure tools, and **test code**.

## Settings

A project can change both levels, whitespace, and proximity for itself or any part of itself, down to a single module.
A value that spans parts takes the most severe level and the narrowest proximity among them.

## The limit

No check that matches strings can tell apart two concepts that share a spelling. `"-n"` is `--no-checkout` to one
command and a count to another, and nothing in the syntax says so. Scoping the check to a file would hide those, and
also the case that matters most: a producer in one file and its consumer in another. The one-function and
one-argument rules take most of that noise; a project with a lot of it can widen its proximity or lower its level.
When a reported value really is two concepts, [name each](magic-strings.md#two-concepts-one-spelling).
