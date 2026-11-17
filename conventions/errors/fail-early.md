# Fail early, fail loudly

Validate where data enters, and refuse to construct anything invalid: the constructor (or the only function that
can build the value) checks, and fails with an error. No exception. Once the value exists, nothing downstream has to
wonder; this is [typed strings](../values/typed-strings.md) doing its job.

It's the same instinct as [strict pseudos](../testing/pseudos.md): an unexpected situation should become an error
immediately, while the cause is still visible, not three layers later.
