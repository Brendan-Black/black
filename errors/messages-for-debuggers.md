# Write error messages for the person debugging

**Convention.** Say what was being attempted, with which input, and what was expected instead. `"Invalid state"`
tells them nothing; `"cannot ship order 4471: status is CANCELLED, expected PAID"` tells them where to look. Keep
secrets and personal data out ([secrets](../values/secrets-in-logs.md)).
