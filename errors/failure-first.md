# Design the failure path first

**Principle.** Failing correctly is arguably more important than succeeding correctly.

> Failure is ALWAYS an option. (Credit: Mythbusters)

The success path runs thousands of times a day; if it's wrong, someone notices by lunch. The failure path runs rarely,
usually under load, at the worst moment, and often for the first time in production. And the costs differ: a loud
failure costs a retry and a bug report, while a quiet wrong answer (a swallowed error, a default that shouldn't have
been returned, a half-written record) costs whatever was built on it before anyone noticed.

So design the failure path as a first-class concern, not as the `catch` block you fill in at the end.
