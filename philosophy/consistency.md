# Follow the code you are in

Everything in this guide is a default. Where the code you are in has an established way of doing something, follow
it. Exception: correctness rules don't yield.

> **"When in Rome, do as the Romans do"** - Saint Ambrose
>
> **"The concept of the road less travelled by does not apply when driving in the slush."** - Brendan Black

One consistent convention, even a mediocre one, beats a codebase where every file reflects whoever last touched it.
Break it and every later reader has to work out whether the difference means anything (it usually doesn't), and every
search has to cover both patterns.

- **Match the surrounding code.** Naming, error handling, null handling, test structure, file layout.
- **Consistency is scoped.** The file first, then the module, then the codebase. A new module with no history gets
  this guide in full.
- **Change a convention everywhere or nowhere.** If a local convention is harmful, change it as its own change, with
  the owners' agreement. A half-finished migration is worse than either the old way or the new one.
- **Correctness is not a convention.** Don't copy a swallowed error or a race condition because the code next door
  has one.
