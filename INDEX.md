# Black: rule index

My style guide, one rule per file, for any language. Read this index; open a rule's file only when the rule is in play
for the code at hand. Each file's title is its row here, and its first paragraph states the rule and its exception,
which is often enough.

- **The folder is the kind.** `philosophy/` frames the rest. `conventions/` yields to the code you are in.
  `correctness/` never yields. The two rule trees are parallel, with the same topic folders inside (`design/`,
  `values/`, `errors/`, `testing/`).
- **Language specifics** live in each language's repo, at the same path under `guide/`:
  `black-java/guide/conventions/values/typed-strings.md` adds Java to `conventions/values/typed-strings.md`. No such
  file means the rule applies as written. Each language's `guide/INDEX.md` lists its files and the tools that enforce
  the rules.
- **Terms** the rules use in a particular sense are defined under [Definitions](#definitions), and linked from the
  rules that use them.
- **Layout** (indentation, braces, line length, import order) is not covered: the formatter decides it, and the build
  enforces it.

## Philosophy

| Rule | Main exception | File |
|---|---|---|
| Follow the code you are in | Correctness rules don't yield | [philosophy/consistency](philosophy/consistency.md) |
| Make the type say it | Not every value earns a type | [philosophy/make-the-type-say-it](philosophy/make-the-type-say-it.md) |
| Design the failure path first | None | [philosophy/failure-first](philosophy/failure-first.md) |
| Every bug becomes one more test case | None | [philosophy/every-bug-a-test](philosophy/every-bug-a-test.md) |
| If you can't test it, fix it | None | [philosophy/untestable-means-fix](philosophy/untestable-means-fix.md) |

## Conventions

### Design

| Rule | Main exception | File |
|---|---|---|
| Interfaces at seams, not everywhere | None | [conventions/design/seams](conventions/design/seams.md) |
| Don't build abstractions nobody uses | Tagging interfaces, whose user is the reader | [conventions/design/no-unused-abstractions](conventions/design/no-unused-abstractions.md) |
| Declare the least interface, keep the identity | You need a concrete type's guarantee | [conventions/design/least-interface](conventions/design/least-interface.md) |
| Don't chain interfaces that aren't definitional | Chain when the child can't stand alone | [conventions/design/no-chained-interfaces](conventions/design/no-chained-interfaces.md) |
| Closed, immutable, least-visible by default | Loosen when a real reason appears | [conventions/design/restrictive-defaults](conventions/design/restrictive-defaults.md) |
| Pass dependencies in; don't reach for them | Genuinely optional dependencies | [conventions/design/constructor-injection](conventions/design/constructor-injection.md) |

### Values

| Rule | Main exception | File |
|---|---|---|
| Nulls don't cross boundaries you own | An announced nullable field; private helpers | [conventions/values/nulls](conventions/values/nulls.md) |
| Name magic strings | Used once, handled in one place, prose, test data | [conventions/values/magic-strings](conventions/values/magic-strings.md) |
| Wrap typed strings | Free text; strings that never leave a function; templated strings | [conventions/values/typed-strings](conventions/values/typed-strings.md) |
| Typed strings cost the check, not the wrapper | Millions held in memory: store the raw form | [conventions/values/typed-strings-cost](conventions/values/typed-strings-cost.md) |
| Validate by default; skip only through a named `trusted` | None | [conventions/values/trusted-path](conventions/values/trusted-path.md) |
| A type's string form is debug output | Small, context-free value wrappers | [conventions/values/debug-strings](conventions/values/debug-strings.md) |

The build's check for magic strings, specified for the language repos that implement it:
[conventions/values/magic-strings-check](conventions/values/magic-strings-check.md). Not a rule; read it only to
change or dispute the check.

### Errors

| Rule | Main exception | File |
|---|---|---|
| Fail early, fail loudly | None | [conventions/errors/fail-early](conventions/errors/fail-early.md) |
| Expected outcomes go in the return type | Only the outcomes callers branch on | [conventions/errors/expected-failures](conventions/errors/expected-failures.md) |
| Write error messages for the person debugging | None | [conventions/errors/messages-for-debuggers](conventions/errors/messages-for-debuggers.md) |
| Errors the reader can't trace get a code | A unique message is enough when the reader has the source | [conventions/errors/messages-for-callers](conventions/errors/messages-for-callers.md) |
| Test the failure path | None | [conventions/errors/test-the-failure-path](conventions/errors/test-the-failure-path.md) |

### Testing

| Rule | Main exception | File |
|---|---|---|
| Internal services run for real, external get a pseudo | Internal ones too heavy to run | [conventions/testing/internal-external](conventions/testing/internal-external.md) |
| Pseudos are strict and inspectable | None | [conventions/testing/pseudos](conventions/testing/pseudos.md) |

## Correctness

### Values

| Rule | Main exception | File |
|---|---|---|
| No HTML, SQL, or shell from string forms or concatenation | A fixed HTML template with every slot escaped | [correctness/values/no-markup-from-strings](correctness/values/no-markup-from-strings.md) |
| No secrets or personal data in logs | None | [correctness/values/secrets-in-logs](correctness/values/secrets-in-logs.md) |

### Errors

| Rule | Main exception | File |
|---|---|---|
| Don't swallow errors | Handle to retry, fall back, or translate | [correctness/errors/dont-swallow](correctness/errors/dont-swallow.md) |
| Leave nothing half-done | None | [correctness/errors/nothing-half-done](correctness/errors/nothing-half-done.md) |

### Testing

| Rule | Main exception | File |
|---|---|---|
| New tests are self-contained | Labelled smoke tests outside the merge gate | [correctness/testing/self-contained](correctness/testing/self-contained.md) |

## Definitions

| Term | Means | File |
|---|---|---|
| Interface | Anything that isn't a concrete worker type: interface, abstract or parent class, trait, wire contract | [definitions/interface](definitions/interface.md) |
| Tagging interface | An empty interface that says what a type is; its user is the reader | [definitions/tagging-interface](definitions/tagging-interface.md) |
| Chained interface | One bound into an inheritance tree it doesn't need to make sense alone | [definitions/chained-interface](definitions/chained-interface.md) |
| Seam | Where one implementation must swap for another, or you promise code you don't control | [definitions/seam](definitions/seam.md) |
| Boundary, edge | Boundary: anything non-private. Edge: where values enter from something you don't control | [definitions/boundary](definitions/boundary.md) |
| Magic string | A literal that carries meaning the code depends on | [definitions/magic-string](definitions/magic-string.md) |
| Typed string | A string that varies but has a shape, or the wrapper type that enforces it | [definitions/typed-string](definitions/typed-string.md) |
| Hermetic | Substantive (could fail for a real reason) and self-contained (depends on your code alone) | [definitions/hermetic](definitions/hermetic.md) |
| Pseudo | A hand-written stand-in, behind the same interface, for what a test can't run for real | [definitions/pseudo](definitions/pseudo.md) |
| Internal and external services | Yours, which the meteor that hits you also takes out; or anything you don't own | [definitions/internal-service](definitions/internal-service.md) |
