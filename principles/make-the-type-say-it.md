# Make the type say it

**Principle.** Put what you know about a value into its type, so the compiler carries it instead of people's memory.
Exception: not every value earns a type.

A string that must look like a SKU, a field that may be absent, a status from a fixed set, a charge that can be
declined: each is knowledge. It lives either in the type, checked at every use, or in someone's head, checked whenever
they remember. Most of the values and errors rules apply this one idea:

- [Absence](../values/nulls.md) is announced in the signature, not discovered at runtime.
- [A fixed set of values](../values/magic-strings.md) becomes an enum.
- [A value with a shape](../values/typed-strings.md) becomes a type that enforces it.
- [An invalid value is never constructed](../errors/fail-early.md), so holding one is proof.
- [Outcomes a caller must handle](../errors/expected-failures.md) become a closed type the compiler makes them handle.

Ask of any value: **could a caller get this wrong without the compiler noticing?** If so, can the type say it instead?
Don't push it past usefulness: a type for every string is the same mistake as an interface for every class. Each rule
says where it stops paying.
