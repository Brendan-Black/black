# Absence doesn't cross boundaries you own

**Convention.** Null, as it happens, is a 4-letter word. Don't let a bare null cross a boundary you own; a boundary is
anything non-private. Where null comes in from the edge (a library, a framework, a column, a caller), convert it to
the language's optional type or a sensible default, and keep it out of your non-private return types. Exception: a
value type's field that announces it may be null; private helpers.

A local that is briefly null inside a function hurts no one, and neither does a private helper that returns null to
the function beside it; a null that travels does. Wrapping those helpers buys nothing: an `isEmpty()` followed by a
`get()` is the same check with more typing.

A nullable field on a pass-through value type is fine as long as it says so; sometimes null genuinely is the value
someone wants to pass. It's safe because the null is announced (an annotation, or the type) and readers get a way
around it (an accessor returning the optional type).

In a language whose type system has no null (an `Option` that can't itself be null), this rule is already enforced:
use the optional type at every boundary and there is nothing more to do.
