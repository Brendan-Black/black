# New tests are self-contained

A new test brings what it needs, gives the same answer every time, doesn't depend on its neighbours, and waits for
events, not durations. Exception: labelled smoke tests, kept outside the merge gate.

This describes a good test; it isn't a checklist. Missing one for a good reason is fine, as long as you know which one
and why. Missing one because the neighbouring tests do is not. Converting an existing suite is its own piece of work.

- **It brings what it needs.** Databases, containers, and servers it talks to are started by the test or its fixture,
  seeded from known data, and cleaned up afterwards. Sharing a container across a suite is fine, and usually necessary
  for speed, as long as each test gets its own isolated state inside it.
- **It gives the same answer every time.** Inject the clock, timezone, locale, and randomness. Injecting a clock must
  not change what it measures: elapsed time (cache ages, timeouts) needs a monotonic source in production, because a
  wall clock jumps, so put the monotonic source behind the interface. Real infrastructure has its own non-determinism
  (the database's `now()`, sequence values, row order without an `ORDER BY`), so assert on what you control.
- **It doesn't depend on its neighbours.** It passes alone, in any order, and in parallel, sharing no mutable state.
  With a real database this isn't free: rolling back a transaction only works when the test and the code under test
  share a connection, and tests that drive the app over the network run on the server's connections. For those, give
  each test its own database or schema, cloned from a seeded template where the database can do that cheaply.
- **It waits for events, not durations.** Wait on a signal: a latch, a readiness check, a polled condition. A sleep
  only makes the race less likely. A timeout on the wait is fine; it is there to fail the test.

[Smoke tests](../../definitions/smoke-test.md) against real environments are fine when labelled and kept out of the
suite that gates a merge. They check your [pseudos](../../conventions/testing/pseudos.md) against the services they
imitate; a pseudo is only as good as your understanding of the real thing.

A fully hermetic test of a database-backed web app has a realistic seed schema and data, a database container
initialized from that seed, and real reads and writes to it, triggered by deterministic network requests against the
running app.
