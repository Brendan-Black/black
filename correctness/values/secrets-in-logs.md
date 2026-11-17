# No secrets or personal data in logs

Passwords, tokens, keys, and personal data never reach a log, an error message, or debug output, directly or through a
generated string form. No exception.

- **Mind generated string forms.** A derived or generated one includes every field, secrets too. Override it for those
  types, or keep secrets in a wrapper whose string form is redacted (`EmailAddress[redacted]`).
- **Don't echo rejected input that may be personal.** "not an email address" is enough.
