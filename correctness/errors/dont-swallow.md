# Don't swallow errors

Handle an error only if you can do something about it: retry, fall back to something genuinely equivalent, or translate
it into a domain outcome. Otherwise let it propagate. Exception: those three are the only exceptions.

```
try { return repository.find(id) } catch (any e) { log.warn("lookup failed", e); return null }
```

That turns an infrastructure outage into "user not found", hands the caller a null, and leaves a warning nobody reads.
When you do wrap, add context and keep the cause: `OrderSyncError("syncing order 4471 to warehouse 7", cause)`.

A designed fallback isn't a swallow, and it doesn't need a log line. If a picture can't be read and the icon is the
intended answer, a comment saying so is enough. Logging every fallback makes noise, and then code to suppress the noise.
Log when someone needs to find out; don't log just to show the handler isn't empty.
