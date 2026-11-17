# Typed string

A string value that varies but has a shape: an email address, a currency code, a SKU, a tenant ID, a version
number. Also the wrapper type that enforces the shape: `Sku` rather than a bare string. Free text (names, descriptions,
search queries) has no shape and isn't one.

An *opaque token* (a session token, an API key, a pagination cursor) has no shape either, but still gets a wrapper,
because nothing may look inside it.

See [wrap typed strings](../conventions/values/typed-strings.md).
