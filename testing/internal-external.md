# Internal and external services

**Convention.** Think of a meteor. If one takes out your datacenter, an internal service is one the same meteor
probably takes down too. We won't define it more strictly than that.

- **Internal services** (your own other services, your own database) run for real, in a container the test starts,
  where that's practical. Where it isn't (too heavy, or dragging half the fleet in behind it), give it a
  [pseudo](pseudos.md).
- **External services** (payment providers, third-party APIs, anything you don't own) get a pseudo.
