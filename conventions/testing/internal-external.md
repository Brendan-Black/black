# Internal services run for real, external get a pseudo

[Internal services](../../definitions/internal-service.md) run for real in the test; external ones get a
[pseudo](pseudos.md). Exception: internal ones too heavy to run.

An internal service runs in a container the test starts. Where that isn't practical (too heavy, or dragging half the
fleet in behind it), give it a pseudo too.
