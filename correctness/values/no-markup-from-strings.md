# No HTML, SQL, or shell from string forms or concatenation

Never generate HTML from a type's string form or by concatenation. Render it through an escaping template engine,
or an explicit, searchable method (`toHtml()`, `render(context)`). SQL and shell: bind parameters and argument lists,
always. Exception: a fixed HTML template with every slot escaped where it is filled.

- **Escaping depends on context.** Element text, attributes, URLs, inline script, and CSS each escape differently, and
  a string form takes no argument to say which.
- **Concatenation drops the type.** `safeHtml + userInput` is a plain string holding unescaped input, and nothing
  downstream can tell.
- **The string form already has a job.** The debugger, the logs, and every assertion message use it ([debug
  strings](../../conventions/values/debug-strings.md)).

So a safe-HTML type hands out its contents through a named method (`asString()`), never its string form. The same
applies, less strictly, to JSON and CSV: use the builder or library.

The rule is for dynamic markup, where structure depends on the data or the slots are many. A fixed document with a
few slots, each escaped where it is filled, is fine. SQL and shell get no such allowance; binding costs nothing.
