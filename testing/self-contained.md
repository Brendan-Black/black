# New tests are self-contained

**Correctness**, for new tests (converting an existing suite is its own piece of work). These are properties to aim
for, a description of a good test rather than a checklist: missing one for a good reason is fine, as long as you know
which one and why. Exception: labelled smoke tests, kept outside the merge gate.

- **It brings what it needs.** Databases, containers, and servers it talks to are started by the test or its fixture,
  seeded from known data, and cleaned up afterwards. (A container the test started is a fixture, not a running
  service.) Sharing a container across a suite is fine, and usually necessary for speed, as long as each test gets its
  own isolated state inside it.
- **It gives the same answer every time.** Inject the clock, timezone, locale, and randomness rather than reading them
  from the environment. Injecting a clock must not change what it measures: elapsed time (cache ages, timeouts) needs
  a monotonic source in production, because a wall clock jumps; put the monotonic source behind the interface, don't
  swap it for the wall clock. Real infrastructure has its own non-determinism (the database's `now()`, sequence
  values, row order without an `ORDER BY`), so assert on what you control, not generated IDs or unordered results.
- **It doesn't depend on its neighbours.** It passes alone, in any order, and in parallel, sharing no mutable state.
  With a real database this isn't free: rolling back a transaction only works when the test and the code under test
  share a connection, and tests that drive the app over the network run on the server's connections. For those, give
  each test its own database or schema. In Postgres, `CREATE DATABASE test_n TEMPLATE seeded_db` makes that cheap.
- **It waits for events, not durations.** A sleep in a test is a race condition with a grace period. Wait on a signal:
  a latch, a readiness check, a polled condition. A timeout on that wait is fine; its job is to fail the test, not to
  synchronise it.

**Smoke tests** against real environments are fine when labelled and kept out of the suite that gates a merge. They
check your [pseudos](pseudos.md) against the services they imitate: a pseudo is only as good as your understanding of
the real thing, and smoke tests show you when that understanding has drifted.

A fully hermetic test of a database-backed web app has a realistic seed schema and data, a Postgres container
initialized from that seed, and real reads and writes to it, triggered by deterministic network requests against the
fully functioning app.
