# Validate by default; skip only through a named `trusted`

Validation is the default. Skipping it is an explicit, named, narrowly visible path, used only where profiling says the
check costs too much. No exception.

On a genuinely hot path, such as bulk-loading millions of rows your own system already validated, a check that stays
expensive may be worth skipping. Don't make validation opt-in: a `Sku(raw).validate()` callers can forget means holding
a `Sku` proves nothing again. Instead:

- `parse(raw)` validates. The default for anything from outside.
- `trusted(raw)` skips validation. Only for values this system already validated, such as its own column. Give it the
  narrowest visibility that works.

"Find usages" on `trusted` then lists every place that assumed validity. Be honest about "already validated": a
migration, a manual fix, or another service writing the same column can each break that assumption.
