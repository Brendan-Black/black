# Errors the reader can't trace get a code

User-facing errors come without a stack trace; at odd hours you hear "it's just not working" and nothing more. Write
errors you can find in the code from whatever the user tells you. Exception: a unique message is enough when the reader
has the source.

- **Give an error a code when its reader can't trace it or will relay it secondhand** (customers, support staff,
  anyone without the source): "E-9". It survives translation and paraphrase, and people read numbers back accurately.
  A code must be unique to its throw site; a shared code is worse than none.
- **Where the reader has the source, a unique message is enough.** It's already the search term. The same goes for a
  usage error that explains itself on the spot, and a console log read by the person debugging.
- **Give it unique wording too, where you can.** A sentence no other error uses ("This order was cancelled, so it
  can't be shipped", not "Something went wrong") is a second way to find it. Don't let it block you; the code already
  makes the error findable.
- **Deliberate misspelling** ("Retreval Error") is a last resort. It's searchable until a person, spellchecker, or AI
  cleanup "fixes" it, and it can look unprofessional.

**The code joins the user's report to your logs.** The user sees the short message and the code; the log gets the
full, [debugging-grade message](messages-for-debuggers.md) with the *same* code. One search finds the throw site and
the log entry, and internals (SQL, stack traces, IDs) stay out of the user's view.

An error code is a [magic string](../values/magic-strings.md): give the codes an enum used by both the log and the
error, whose string form is the code (`"E-9"`, [debug strings](../values/debug-strings.md)). Searching for "E-9" lands
on the constant, and "find usages" on every place it is raised and logged.

> **A side note.** If unique messages are getting hard to write, the module is too big. Keep the scope tight, or split
> it into pieces small enough to name their own failures.
