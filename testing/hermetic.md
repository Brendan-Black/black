# Hermetic testing: substantive and self-contained

**Principle.** Tests are **substantive** (they exercise real behaviour and could fail for a real reason) and
**self-contained** (they bring everything they need, so the result depends on your code and nothing else).

The rest of this guide builds toward this. Seams are what let a pseudo stand in; typed values and validation shrink
what a test has to cover; error codes and loud failures tell you what to reproduce. Containers made real
infrastructure practical inside a test, and AI has made the remaining cost, writing and maintaining pseudos, much
cheaper. There is less excuse than there used to be.

- The goal: [every bug becomes one more test case](every-bug-a-test.md).
- What it looks like: [self-contained](self-contained.md).
- What runs for real and what is faked: [internal and external services](internal-external.md).
- How fakes behave: [pseudos](pseudos.md).
- When it seems impossible: [if you can't test it, fix it](untestable-means-fix.md).
