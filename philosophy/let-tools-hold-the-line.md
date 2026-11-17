# Let the build enforce what a tool can check

Rules enforced by the build survive; rules that live only in prose erode one review at a time. Where a tool can check
a rule, wire it into the build and let it fail, so reviewers spend attention on what tools can't see. Exception: what
only review sees (below).

Each language's `guide/INDEX.md` lists its tools for this guide's rules. Layout is always the formatter's job,
enforced in the build; don't argue about it in review.

What no tool catches well, and review is for: whether an interface sits at a real seam, whether a signature has lost
the identity of its argument, whether debug output is context-free, what ends up in a log, and whether a pseudo is
strict.
