# If you can't test it, fix it

**Principle.** If the only way to test some code is against something already live (a shared staging database, a
colleague's service, a third-party sandbox), the problem is the code, not the test.

Its dependencies are welded in: a client constructed inside a function, a static singleton, a connection string read
from a global. That coupling also makes the code hard to change, reuse, and reason about; the test is just where it
hurts first. The fix is a [seam](../design/seams.md) and [constructor injection](../design/constructor-injection.md),
not a test that reaches out into the world.
