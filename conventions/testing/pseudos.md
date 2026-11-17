# Pseudos are strict and inspectable

A [pseudo](../../definitions/pseudo.md) behaves plausibly, honours the contract, and **fails loudly when the contract
is violated**. It records what it was asked, for the test to inspect. No exception.

Its data may be made up, but it is shaped like the real thing (`SomeRecord([1, 2, 3], "John Smith")`). A stand-in that
only satisfies the compiler (returning nulls, empty collections, or whatever makes the code run) is not a pseudo. It
tests nothing but the type system.

A pseudo `HttpGateway` holds a map of canned responses by URI and a list of the requests it received:

- `fetch(request)` records the request, returns the canned response, and **fails** with "no canned response for
  <uri>" when there is none. Returning a default for an unexpected call would hide the bug where you fetched the wrong
  URL.
- `received()` returns a copy of what the code under test asked for, in order.

Assert on *what happened*, not *how*. A request that leaves your system is an outcome: the payment provider was asked
to charge that card, whatever route your code took. Which of your own functions were called, in what order, how many
times, is not, and assertions on it break in a refactor that changes no behaviour. Assert on the order of `received()`
only when the order is part of the contract (authorise before capture).

A few smoke tests against the real services check that pseudos haven't drifted from them
([self-contained](../../correctness/testing/self-contained.md)).
