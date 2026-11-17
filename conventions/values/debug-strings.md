# A type's string form is debug output

A type's [string form](../../definitions/string-form.md) is **debug output for a developer**: what the debugger, a
failed assertion, or a stray log line shows. Make it useful to that reader; don't parse it, persist it, compare
against it, or depend on its format. Exception: small, context-free value wrappers.

Implicit calls to it don't show up in "find usages", the same problem [magic strings](magic-strings.md) solves.

## The exception: small value wrappers

A thin wrapper whose identity *is* a string may produce that string as its **[canonical, context-free
form](../../definitions/string-form.md)**. An error code (`"E-9"`, [messages for
callers](../errors/messages-for-callers.md)) reads the same in a log line, an exception, and a support ticket. So does
a correlation ID that logs as the ID, not `CorrelationId[value=...]`.

Keep these wrappers small. If the output should vary by destination, it isn't context-free.

## Don't assert on it

A test that compares debug output breaks when someone improves the format, and passes when the fields it should check
are wrong. Assert on the fields.
