# Internal services run for real, external get a pseudo

[Internal services](../../definitions/internal-service.md) (your own services and databases) run for real in the test;
external ones, anything you don't own, get a [pseudo](pseudos.md). Exception: internal ones too heavy to run.

An internal service runs in a container the test starts, where that's practical. Where it isn't (too heavy, or
dragging half the fleet in behind it), give it a pseudo too.
