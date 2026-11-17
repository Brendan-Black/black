# Don't build abstractions nobody uses

Interfaces are important; generality is not. **An abstraction nobody uses is useless by definition**, and every
reader still has to understand it. Exception: [marker interfaces](../../definitions/marker-interface.md), whose user
is the reader.

The usual offenders:

- An interface with one implementation and no test that substitutes it, "in case we need to swap it later."
- A type parameter only ever instantiated with one type.
- A strategy, hook, or plugin point with exactly one strategy.
- A config flag or overload no caller passes a different value to.
- A base class extracted from one subclass.

The second implementation tells you what the abstraction should be. Build it then.
