# Leave nothing half-done

A failure partway through an operation leaves the system as it was, not somewhere in between. No exception.

Use transactions where you have them. Where you don't, build the complete result first and commit it in one step
(write a temporary file, then rename it over the original), and make operations that cross service boundaries safe to
retry.
