# Test the failure path

Cover the failure path on purpose: give [pseudos](../testing/pseudos.md) canned failures (timeouts, 503s, malformed
responses) alongside canned successes, and assert that the system fails the way you designed it to. No exception. It is
the path most likely to go untested, which is exactly backwards.
