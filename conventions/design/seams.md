# Interfaces at seams, not everywhere

"Program to an interface, not an implementation", applied where it pays: interfaces go at the
[seams](../../definitions/seam.md) of a system, not in front of every class. Everything else is a concrete type. No
exception: a type with no seam simply doesn't get one.

A type sits at a seam, and gets an interface, when at least one holds:

- **It touches the outside world.** Network, database, clock, randomness, environment. Hermetic tests replace these
  with [pseudos](../testing/pseudos.md), and a pseudo needs an interface to stand behind: `HttpGateway`,
  `UserRepository`, `Clock`. The local disk usually doesn't count: a temporary folder the test creates is the real
  thing, brought along. Add one only when a test needs something a real folder can't do, such as a disk that fills up
  part way.
- **It is a contract across a module or team boundary.** If someone else compiles against it, the interface is where
  you write down what you promise, separately from what you happen to do today.
- **It has, or clearly will have, more than one implementation.** Strategies, policies, pluggable formatters. "Clearly
  will" means you can name the second implementation, not that you can imagine one.

A service that coordinates two repositories and applies some rules doesn't need a `FooService` and a
`FooServiceImpl`. Wire the real thing and test the real thing, with pseudos only at the boundaries beneath it. Value
types rarely need one; they *are* the contract.

## Write it from the caller's side

An interface extracted after the fact tends to be the class's surface (its setters, lifecycle hooks, the convenience
method one caller needed), not a designed contract. When you see a seam coming, write the interface first, shaped by
what callers need.

When in doubt, start with a concrete type. When a real seam appears (a second implementation, a test that needs a
pseudo, a consumer in another module), extract the interface then, from what that caller needs rather than the type's
public methods. Indirection costs every reader a hop through "go to definition"; make sure something pays
it back.
