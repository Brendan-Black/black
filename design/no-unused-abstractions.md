# Don't build abstractions nobody uses

**Convention.** Interfaces are important; generality is not. **An abstraction nobody uses is useless by definition**,
and worse: every reader must understand it, every change must respect it, and it looks like a promise the code isn't
making. Exception: marker interfaces, whose user is the reader.

The usual offenders:

- An interface with one implementation and no test that substitutes it, "in case we need to swap it later."
- A type parameter only ever instantiated with one type.
- A strategy, hook, or plugin point with exactly one strategy.
- A config flag or overload no caller passes a different value to.
- A base class extracted from one subclass.

Each is a bet on a future that usually doesn't arrive, and rarely looks the way you guessed when it does. The second
implementation tells you what the abstraction should be; build it then.
