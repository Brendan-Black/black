# If you can't test it, fix it

If the only way to test some code is against something already live (a shared staging database, a colleague's service,
a third-party sandbox), the problem is the code, not the test. No exception. You have you ask yourself how you even got
into this situation in the first place before you can even begin to consider if your logic in-situ is good.

> "If I can't read it, it's wrong." - Linda Streeper (my mother, on my handwriting as a child)

It usually happens when dependencies are welded in: a client constructed inside a function, a static singleton, a
connection string read from a global. The fix is a [seam](../conventions/design/seams.md) and
[passing dependencies in](../conventions/design/constructor-injection.md), not a test that reaches out into the world.
