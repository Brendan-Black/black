# String form

The string a language produces for a value unasked: what string interpolation, logging placeholders, and the debugger
call.

A *canonical, context-free* string form is correct wherever it lands: nothing in it needs escaping, and concatenating
it with anything does no harm. An error code (`"E-9"`) has one.

See [a type's string form is debug output](../conventions/values/debug-strings.md).
