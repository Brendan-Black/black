# Internal and external services

An *internal service* is one of your own: your other services, your own database. An *external service* is anything you
don't own: payment providers, third-party APIs.

Think of a meteor. If one takes out your datacenter, an internal service is one the same meteor probably takes down
too. We won't define it more strictly than that.

See [internal services run for real, external get a pseudo](../conventions/testing/internal-external.md).
