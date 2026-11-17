# Correctness doesn't defer to local habit

Most of this guide is convention, and yields to the surrounding code. A handful is correctness, and doesn't: **if
following the local pattern would make your code wrong, insecure, or flaky, it gets no deference.** No exception.

**Convention (follow the local pattern):** where interfaces go and whether a service gets an `Impl`; how narrowly
collaborators are declared; whether interfaces are chained; default finality, mutability, visibility, and how
dependencies are injected; how absence is represented; how constants are named and grouped; which shaped strings get a
type; what debug output prints; result types vs exceptions for expected failures, and the error-code scheme; test
structure, naming, and layout.

**Correctness (do it right regardless):**

- [Don't swallow errors.](../correctness/errors/dont-swallow.md) Copying `catch (any e) { return null }` copies the bug.
- [No HTML, SQL, or shell from string forms or concatenation.](../correctness/values/no-markup-from-strings.md) That's
  an injection hole, not a style choice.
- [No secrets or personal data in logs](../correctness/values/secrets-in-logs.md), directly or through generated debug
  output.
- [Leave nothing half-done.](../correctness/errors/nothing-half-done.md) A non-atomic write is a data-corruption bug
  however common it is nearby.
- [New tests are self-contained.](../correctness/testing/self-contained.md) A test that passes only when something else
  happens to be running is flaky, and flakiness is a defect. Write *your* new tests this way even if the neighbours
  aren't; converting the existing suite is its own piece of work.

Where the line is unclear, ask whether a reviewer seeing the local pattern for the first time would call it a bug. If
so, it's correctness.
