# Pseudos are strict and inspectable

**Convention.** A *pseudo-implementation* (*pseudo*) stands in for something the test can't run for real. It is
hand-written, implements the same interface, behaves plausibly, honours the contract, and **fails loudly when the
contract is violated**. It records what it was asked, for the test to inspect. No exception.

Its data may be made up, but it is shaped like the real thing (`SomeRecord([1, 2, 3], "John Smith")`). A stand-in that
only satisfies the compiler (returning nulls, empty collections, or whatever makes the code run) is not a pseudo. It
tests nothing but the type system.

A pseudo `HttpGateway` holds a map of canned responses by URI and a list of the requests it received:

- `fetch(request)` records the request, returns the canned response, and **fails** with "no canned response for
  <uri>" when there is none. A pseudo that returns a default for an unexpected call hides the bug where you fetched
  the wrong URL; one that fails loudly turns it into a test failure with a useful message.
- `received()` returns a copy of what the code under test asked for, in order.

The inspection surface is for asserting on *what happened*, not *how*. A request that leaves your system is an outcome
in its own right: the payment provider really was asked to charge that card, whatever route your code took. So
`received()` belongs with the resulting state. Internal choreography doesn't: which of your own functions were called,
in what order, how many times. Assertions on outcomes survive a refactor; assertions on internal call sequences break
when calls are restructured even if behaviour didn't change. Assert on the order of `received()` only when the order
is part of the contract (authorise before capture).

A few smoke tests against the real services check that pseudos haven't drifted from them
([self-contained](self-contained.md)).
