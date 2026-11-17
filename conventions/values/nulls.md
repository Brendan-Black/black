# Nulls don't cross boundaries you own

Null, as it happens, is a 4-letter word. Don't let a bare null cross a [boundary](../../definitions/boundary.md) you
own. Where null comes in from the edge (a library, a framework, a column, a caller), convert it to the language's
optional type or a sensible default, and keep it out of your non-private return types. Exception: a value type's field
that announces it may be null; private helpers.

A local that is briefly null inside a function hurts no one, and neither does a private helper that returns null to
the function beside it. Wrapping those buys nothing: an `isEmpty()` followed by a `get()` is the same check with more
typing.

A nullable field on a pass-through value type is fine as long as it says so (an annotation, or the type) and gives
readers a way around it (an accessor returning the optional type). Sometimes null is the value someone wants to pass.

In a language with no null (an `Option` that can't itself be null), the type system already enforces this.
