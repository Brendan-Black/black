# No HTML, SQL, or shell from string forms or concatenation

**Correctness.** Never generate HTML from a type's string form or by concatenation. Render it through an escaping
template engine, or an explicit, searchable method (`toHtml()`, `render(context)`). SQL and shell: bind parameters and
argument lists, always. Exception: a fixed HTML template with every slot escaped where it is filled.

- **Escaping depends on context.** Element text, attributes, URLs, inline script, and CSS each escape differently. A
  string form takes no arguments, so it can't know which one it is producing.
- **The safety lives in the type, and the type evaporates.** A `SafeHtml` that promises escaped contents stops
  promising once concatenated: `safeHtml + userInput` is a plain string holding unescaped input, and nothing
  downstream can tell.
- **It already has a job.** The debugger, the logs, and every assertion message use the string form too
  ([debug strings](debug-strings.md)). Either your page renders debug text or your debugger shows markup.

GWT's `SafeHtml` chose `asString()` over `toString()` for exactly these reasons. The same applies, more gently, to JSON
and CSV: use the builder or library.

This line is for dynamic markup, where structure depends on the data or the slots are many. A fixed document with a
few slots, each escaped where it is filled, is fine: it reads like what it produces. SQL and shell get no such
allowance; bind parameters and argument lists cost nothing.
