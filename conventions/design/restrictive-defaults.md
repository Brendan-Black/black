# Closed, immutable, least-visible by default

Start every type at the most restrictive setting the language offers. Exception: loosen it when a real reason appears,
not before.

- **Closed to extension.** A subclass of a class not built for one is coupled to its internals. This is the "worker"
  behind a [seam](seams.md). When something genuinely needs to extend it, open it up and document what subclasses may
  rely on.
- **Immutable.** Fields can't be reassigned; collections are copied on the way in and never handed out mutable. An
  immutable value can be shared across threads, cached, and used as a map key without anyone having to think. Mutable
  state is fine where it's the point (a builder, an accumulator inside a function, the recorder in a pseudo); keep it
  local and don't let it escape.
- **Least visibility.** Public is a promise to every caller in every module. Make public what other modules need and
  nothing else. Narrow visibility is also how you fence off escape hatches such as
  [`trusted`](../values/trusted-path.md).
