# Make the type say it

Put what you know about a value into its type, so the compiler carries it instead of people's memory. Exception: not
every value earns a type.

A string that must look like a SKU, a field that may be absent, a status from a fixed set, a charge that can be
declined: each is knowledge. It lives in the type, checked at every use, or in someone's head, checked when they
remember. Most of the values and errors rules are this one idea:

- [Absence](../conventions/values/nulls.md) is announced in the signature, not discovered at runtime.
- [A fixed set of values](../conventions/values/magic-strings.md) becomes an enum.
- [A value with a shape](../conventions/values/typed-strings.md) becomes a type that enforces it.
- [An invalid value is never constructed](../conventions/errors/fail-early.md), so holding one is proof.
- [Outcomes a caller must handle](../conventions/errors/expected-failures.md) become a closed type the compiler makes
  them handle.

Ask of any value: **could a caller get this wrong without the compiler noticing?** But a type for every string is the
same mistake as an interface for every class. Each rule says where to stop.
