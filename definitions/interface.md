# Interface

Anything that isn't a concrete worker type: an interface, an abstract or parent class, a trait, a wire contract. The
guide uses the one word for all of them.

The *worker type* is the concrete one that does the work (`SmtpEmailSender`); the *role* is the interface it fills
(`EmailSender`).

See [interfaces at seams](../conventions/design/seams.md) and [the least
interface](../conventions/design/least-interface.md).
