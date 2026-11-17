# Fail early, fail loudly

Validate where data enters, and refuse to construct anything invalid: the constructor (or the only function that
can build the value) checks, and fails with an error. No exception.

Once the value exists, nothing downstream has to wonder ([typed strings](../values/typed-strings.md)). As with
[strict pseudos](../testing/pseudos.md), the unexpected becomes an error while the cause is still visible.
