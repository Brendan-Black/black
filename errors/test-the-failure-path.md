# Test the failure path

**Convention.** The failure path is the one most likely to go untested, which is exactly backwards. Cover it on
purpose: give [pseudos](../testing/pseudos.md) canned failures (timeouts, 503s, malformed responses) alongside canned
successes, and assert that the system fails the way you designed it to.
