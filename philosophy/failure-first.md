# Design the failure path first

Design the failure path as a first-class concern, not as the `catch` block you fill in at the end: failing correctly is
arguably more important than succeeding correctly. No exception.

> Failure is ALWAYS an option. (Credit: Mythbusters)

The success path runs thousands of times a day; if it's wrong, someone notices by lunch. The failure path runs rarely,
usually under load, at the worst moment, and often for the first time in production.

> "Failing properly is arguably more important than succeeding properly."

The costs differ, too: a loud failure costs a retry and a bug report, while a quiet wrong answer (a swallowed error, a
default that shouldn't have been returned, a half-written record) costs whatever was built on it before anyone noticed.

