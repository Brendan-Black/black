# Magic string

A literal that carries meaning the code depends on: a status, a header name, a config key, a JSON field, a role, an
event type. Spell it differently in two places and the code is wrong, with nothing to say so.

Prose (log and error messages, UI copy) and test data are not magic strings. See [name magic
strings](../conventions/values/magic-strings.md).
