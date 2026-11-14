# Declare the least interface, keep the identity

**Convention.** Declare variables, parameters, and injected dependencies as the minimal sensible shape the task needs:
the map interface, not the hash map; the role (`EmailSender`), not the worker filling it (`SmtpEmailSender`).
Exception: you depend on a guarantee only the concrete type gives (such as an insertion-ordered map's iteration order).

## Least is not "only what is used"

Given `interface HasId { id() }` and `interface CacheKey<T> extends HasId`, this is right:

```
invalidateUser(key: CacheKey<User>) { userCache.evict(key.id()) }
```

Narrowing to what the body calls, `invalidateUser(key: HasId)` or `invalidateUser(key: CacheKey<?>)`, still compiles,
since the body only calls `id()`. But the identity is lost, and whoever comes after is left guessing at the intent. The
operation was never on *keys*; it is on *user keys*.

**Narrow the capabilities you require, not the identity of the thing you operate on.** The test is not "does it still
compile?" but **"would this method correctly handle every value its signature accepts, in the context of the
surrounding code?"**
