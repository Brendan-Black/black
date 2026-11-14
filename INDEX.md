# Black: rule index

My style guide, one rule per file, for any language. Read this index; open a rule's file only when the rule is in play
for the code at hand. Each file states its rule, kind, and exception in its first paragraph, which is often enough.

- **Kinds.** *Principle* frames the rest. *Convention* yields to the code you are in. *Correctness* never yields
  ([why](principles/correctness-vs-convention.md)).
- **Language specifics** live in each language's repo, at the same path under `guide/`: `black-java/guide/values/typed-strings.md`
  adds Java to `values/typed-strings.md`. No such file means the rule applies as written. Each language's
  `guide/INDEX.md` lists its files and the tools that enforce the rules.
- **"Interface"** means more than a keyword: anything that isn't a concrete worker type. An abstract class, a parent
  class, a trait, a wire contract. The guide assumes you know dependency injection, its strengths and its limits.
- **Layout** (indentation, braces, line length, import order) is not covered: the formatter decides it, and the build
  enforces it.

| Rule | Kind | Main exception | File |
|---|---|---|---|
| Follow the code you are in | Principle | Correctness rules don't yield | [principles/consistency](principles/consistency.md) |
| Correctness doesn't defer to local habit | Principle | None | [principles/correctness-vs-convention](principles/correctness-vs-convention.md) |
| Make the type say it | Principle | Not every value earns a type | [principles/make-the-type-say-it](principles/make-the-type-say-it.md) |
| Let the build enforce what a tool can check | Principle | What only review sees | [principles/let-tools-hold-the-line](principles/let-tools-hold-the-line.md) |
| Interfaces at seams, not everywhere | Convention | None needed where nothing is swapped or promised | [design/seams](design/seams.md) |
| Don't build abstractions nobody uses | Convention | Marker interfaces, whose user is the reader | [design/no-unused-abstractions](design/no-unused-abstractions.md) |
| Declare the least interface, keep the identity | Convention | You need a concrete type's guarantee | [design/least-interface](design/least-interface.md) |
| Don't chain interfaces that aren't definitional | Convention | Chain when the child can't stand alone | [design/no-chained-interfaces](design/no-chained-interfaces.md) |
| Closed, immutable, least-visible by default | Convention | Loosen when a real reason appears | [design/restrictive-defaults](design/restrictive-defaults.md) |
| Inject through constructors | Convention | Genuinely optional dependencies | [design/constructor-injection](design/constructor-injection.md) |
| Nulls don't cross boundaries you own | Convention | An announced nullable field; private helpers | [values/nulls](values/nulls.md) |
| No nullable optional in a constructor | Correctness | None | [values/no-optional-in-constructor](values/no-optional-in-constructor.md) |
| Name magic strings | Convention | Used once, handled in one place, prose, test data | [values/magic-strings](values/magic-strings.md) |
| Wrap typed strings | Convention | Free text; strings that never leave a function; templated strings | [values/typed-strings](values/typed-strings.md) |
| Typed strings cost the check, not the wrapper | Convention | Millions held in memory: store the raw form | [values/typed-strings-cost](values/typed-strings-cost.md) |
| Validate by default; skip only through a named `trusted` | Convention | None | [values/trusted-path](values/trusted-path.md) |
| A type's string form is debug output | Convention | Small, context-free value wrappers | [values/debug-strings](values/debug-strings.md) |
| No HTML, SQL, or shell from string forms or concatenation | Correctness | A fixed HTML template with every slot escaped | [values/no-markup-from-strings](values/no-markup-from-strings.md) |
| No secrets or personal data in logs | Correctness | None | [values/secrets-in-logs](values/secrets-in-logs.md) |
| Design the failure path first | Principle | None | [errors/failure-first](errors/failure-first.md) |
| Fail early, fail loudly | Convention | None | [errors/fail-early](errors/fail-early.md) |
| Don't swallow errors | Correctness | Handle to retry, fall back, or translate | [errors/dont-swallow](errors/dont-swallow.md) |
| Expected outcomes go in the return type | Convention | Only the outcomes callers branch on | [errors/expected-failures](errors/expected-failures.md) |
| Error messages say what, with which input, expected what | Convention | None | [errors/messages-for-debuggers](errors/messages-for-debuggers.md) |
| Errors the reader can't trace get a code | Convention | A unique message is enough when the reader has the source | [errors/messages-for-callers](errors/messages-for-callers.md) |
| Leave nothing half-done | Correctness | None | [errors/nothing-half-done](errors/nothing-half-done.md) |
| Test the failure path | Convention | None | [errors/test-the-failure-path](errors/test-the-failure-path.md) |
| Tests are substantive and self-contained | Principle | None | [testing/hermetic](testing/hermetic.md) |
| Every bug becomes one more test case | Principle | None | [testing/every-bug-a-test](testing/every-bug-a-test.md) |
| If you can't test it, fix it | Principle | None | [testing/untestable-means-fix](testing/untestable-means-fix.md) |
| Internal services run for real, external get a pseudo | Convention | Internal ones too heavy to run | [testing/internal-external](testing/internal-external.md) |
| Pseudos are strict and inspectable | Convention | None | [testing/pseudos](testing/pseudos.md) |
| New tests are self-contained | Correctness | Labelled smoke tests outside the merge gate | [testing/self-contained](testing/self-contained.md) |
