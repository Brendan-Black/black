# Chained interface

An interface bound into an inheritance tree it doesn't need in order to make sense on its own: `User extends
CacheableObject`. The chain is *definitional* when the child is meaningless without the parent: `Eukaryote extends
Cell` (a eukaryote with no cell is no eukaryote).

See [don't chain interfaces that aren't definitional](../conventions/design/no-chained-interfaces.md).
