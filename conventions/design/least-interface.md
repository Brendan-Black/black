# Declare the least interface, keep the identity

Declare variables, parameters, and injected dependencies as the least type the task needs: the map interface, not the
hash map; the [role](../../definitions/interface.md) (`EmailSender`), not the worker filling it (`SmtpEmailSender`).
Exception: you depend on a guarantee only the concrete type gives, such as an insertion-ordered map's iteration order.

## Least is not "only what is used"

Given `interface HasId { id() }` and `interface CacheKey<T> extends HasId`, this is right:

```
invalidateUser(key: CacheKey<User>) { userCache.evict(key.id()) }
```

`invalidateUser(key: HasId)` and `invalidateUser(key: CacheKey<?>)` also compile, since the body only calls `id()`.
But the identity is lost, and whoever comes after is left guessing at the intent. The operation is on *user keys*, not
*keys*.

**Narrow the capabilities you require, not the identity of the thing you operate on.** That it still compiles proves
nothing. Ask: **"would this method correctly handle every value its signature accepts, in the context of the
surrounding code?"**
