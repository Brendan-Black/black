# Don't chain interfaces that aren't definitional

**Convention.** Don't bake one layer's concern into another layer's types. The usual way is a *chained interface*:
one bound into an inheritance tree it doesn't need in order to make sense on its own. Exception: chain when the child
can't stand alone.

`interface User extends CacheableObject` (with `getCacheKey()` narrowed to `UserId`) isn't wrong today; it paints you
into a corner. A user is not *definitionally* cacheable. Caching is the cache layer's concern, and baking it into
`User` means a user can't travel anywhere without the cache coming along. The day someone wants users shared across
services without the cache, you split `User` into a `UserConcept` and a leftover `User extends CacheableObject,
UserConcept`, and every further concern multiplies the combinations. Better not to play:

```
interface User { username() }        // a seam: tests stand in a PseudoUser
userCache: Cache<UserId, User>       // the cache knows about users; users know nothing of the cache
```

This isn't [speculative generality](no-unused-abstractions.md). Declining to chain builds nothing and costs nothing;
you aren't betting on a future, only declining to close one off. In a legacy codebase, the best you can often do is an
empty interface that unions the two types.

Chain where the extending interface makes no sense alone: `Eukaryote extends Cell` (a eukaryote with no cell is no
eukaryote). When the relationship is definitional, chain. When it **is not**, take a second look.
