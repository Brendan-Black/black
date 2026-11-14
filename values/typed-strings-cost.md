# Typed strings on hot paths

**Convention.** The wrapper is not the cost; the check is. Keep the wrapper, and make the check cheap.

- **Keep the wrapper.** In a managed or compiled runtime, passing a small wrapper around costs close to nothing, so
  hot code is no reason to go back to raw strings.
- **Validate once, at the edge.** Never re-validate something already typed. If construction runs the check twice
  (a parse function and a validating constructor), that check needs to be cheap.
- **Choose the check by how often it runs.** A regex is fine for startup and config. Per request or in a loop, write
  the character loop: regex engines tend to allocate per call, a hand-written loop needn't.
- **Never re-derive from the string.** A type that needs its parsed form holds it, or uses a cheap test. Parsing a URI
  inside `isLocal()` measured about 50 times the cost of checking the prefix.
- **Save [`trusted`](trusted-path.md) for checks that stay expensive by hand**, such as parsing URIs or dates in bulk.
- **For millions of values held in memory, store the raw form.** Keep a raw array inside one type and return the
  wrapper from the accessor; millions of separate objects cost memory and, where there is a GC, tracing.

Measured numbers for each language live in its guide.
