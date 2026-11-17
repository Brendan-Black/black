# Pseudo

Short for *pseudo-implementation*: a hand-written stand-in for something a test can't run for real, implementing the
same [interface](interface.md) as the real thing. `PseudoHttpGateway` stands in for `HttpGateway`.

What makes one good: [pseudos are strict and inspectable](../conventions/testing/pseudos.md). When to use one:
[internal services run for real, external get a pseudo](../conventions/testing/internal-external.md).
