# No optional in a constructor

**Correctness.** Where the language's optional wrapper can itself be null, it never appears in a constructor's
parameters, a record's components included. No exception, and no deference to the code around you. Languages whose
optional can't be null don't have the problem.

The optional wrapper is a return-type tool. As a parameter it is one more thing that can itself be null, so the
"optional" value has three states (present, empty, null) and the type admits to two. At scale: `Address(empty(),
empty(), of(city), empty())` at every construction site, every fixture and mapper wrapping and unwrapping, the
ceremony burying the data, and still nothing stopping a bare null passed for one of them.

Take the plain value, announced nullable, and hand out the optional from an accessor (see [nulls](nulls.md)). Where
the language offers no nullable annotation, split the constructor (one with the value, a named factory without it), or
use a named default with an optional accessor (a zero duration for an unknown length, read through `knownLength()`).
A default is a weaker promise than an annotation, so keep the accessor next to it.
