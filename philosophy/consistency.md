# Follow the code you are in

Everything in this guide is a default, not an override. Where the code you are in has an
established way of doing something, follow it. Exception: correctness rules don't yield
([correctness-vs-convention](correctness-vs-convention.md)).

> **"When in Rome, do as the Romans do"** - Saint Ambrose
>
> **"The concept of the road less travelled by does not apply when driving in the slush."** - Brendan Black

One consistent convention, even a mediocre one, is easier to read, search, and change than a codebase where every file
reflects whoever last touched it. Breaking convention costs everyone after you: every reader must work out whether the
difference means anything (it usually doesn't), every search must cover both patterns, and every change must pick one.
The worn path has traction; striking out on your own is how you end up in the ditch.

- **Match the surrounding code.** Naming, error handling, null handling, test structure, file layout.
- **Consistency is scoped.** The file first, then the module, then the codebase. A new module with no history is where
  this guide applies in full.
- **Change conventions deliberately.** If a local convention is harmful, change it everywhere, as its own change, with
  agreement from the code's owners, not in the one file you happen to be in. A half-finished migration is worse than
  either the old way or the new one.
- **Correctness is not a convention.** You don't copy a swallowed error or a race condition because the code next door
  has one.
