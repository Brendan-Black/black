# black

My style guide, language-agnostic: one rule per file, indexed in [`INDEX.md`](INDEX.md).

Each language has its own repo with the tooling that enforces the guide (formatter, compiler warnings, hard-rule
checks) and a `guide/` folder holding only what that language adds, at the same paths as here:

- [black-java](https://github.com/Brendan-Black/black-java): Eclipse formatter profile, ECJ warnings, a Gradle plugin.
- [black-rust](https://github.com/Brendan-Black/black-rust): rustfmt and Cargo lints, and `cargo black`.

## Why it's split up

So an AI can follow it without reading all of it. The index is the only file that needs to be in context: every rule
in one line, with its kind, exception, and file. Each file opens with the rule in one paragraph, so reading only that
paragraph is usually enough, and the rest is there when the rule is contested.

## Using it in a project

With this repo checked out at `~/github/black` and the language repo at its usual place, put this in the project's
`CLAUDE.md`. `@` imports a file into Claude Code's context, so only the two indexes are loaded up front:

```markdown
# Style

Follow the black style guide. Its rule index, and the Java specifics:

@~/github/black/INDEX.md
@~/github/java/black-java/guide/INDEX.md

Open a rule's file (paths are relative to ~/github/black, and to the language's guide/) only when the rule is in play.
```

For Rust, the second import is `@~/github/rust/black-rust/guide/INDEX.md`.

## Writing a rule

- One rule per file. The title is the rule, word for word as its row in `INDEX.md`.
- The folder is the kind: `philosophy/` (flat), or `conventions/` and `correctness/`, which share the topic folders
  (`design/`, `values/`, `errors/`, `testing/`). Moving a rule between them changes its kind; nothing in the file
  says it.
- The first paragraph states the rule and ends with `Exception: …` or `No exception.` Someone who reads only that
  paragraph should apply the rule correctly.
- Detail, examples, and the case for the rule come after. A tool's full specification goes in its own reference file
  (like `conventions/values/magic-strings-check.md`), not in the rule.
- Language-neutral here: pseudocode or none. Code in a real language goes in that language's `guide/`.
- Keep the voice; cut the repetition. Link instead of restating another rule.

## Writing a definition

A term used by more than one rule, or one a reader might take another way, gets a file in `definitions/`: the title is
the term, and the first paragraph says what it means here. Add a row to the index's definitions table, and link the
term where a rule relies on its meaning rather than defining it again. Say what the word means; what to do about it
belongs in a rule.
