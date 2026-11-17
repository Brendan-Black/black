# Smoke test

A test against a real environment (staging, a provider's sandbox) instead of what the test brings itself. It can't be
[hermetic](hermetic.md), and isn't meant to be: it checks that [pseudos](pseudo.md) still match the services they
imitate.

See [new tests are self-contained](../correctness/testing/self-contained.md), where it is the exception.
