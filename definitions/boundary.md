# Boundary, edge

A *boundary* is anything non-private: a signature that code outside the function's file, type, or module can call. A
boundary *you own* is one whose signature you write. The *edge* is where values enter your code from something you
don't control: a request, a file, a column, a library, a framework, a caller.

See [nulls don't cross boundaries you own](../conventions/values/nulls.md) and [wrap typed
strings](../conventions/values/typed-strings.md).
