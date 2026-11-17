# Hermetic

A hermetic test is *substantive*: it exercises real behaviour and could fail for a real reason; and *self-contained*:
it brings everything it needs, so its result depends on your code and nothing else.

A stand-in that only satisfies the compiler makes a test that isn't substantive. A test that reads the staging
database, or passes only after its neighbour ran, isn't self-contained.

See [new tests are self-contained](../correctness/testing/self-contained.md) for the four properties that make a test
self-contained.
