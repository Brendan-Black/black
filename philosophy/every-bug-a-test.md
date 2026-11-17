# Every bug becomes one more test case

You won't think of every edge case. When someone finds the one you missed, **reproducing it should be as quick as
adding that exact input to a list.** No exception.

The fixtures, seed data, and pseudos already exist. Add the case, watch it fail, fix the code, and leave the case in
as a regression test, with a comment saying where it came from:

```
"alice+tag@example.com",            // reported in production, 2026-03: plus-addressing was rejected
"Robert'); DROP TABLE Students;--", // https://xkcd.com/327/
```

Once [little Bobby Tables](https://xkcd.com/327/) has visited, he gets a line too.

An outside-world case ("the payment provider returned a 200 with an empty body") is one more canned response for a
pseudo.

If reproducing a reported bug takes a day of setup or a live environment, the test infrastructure isn't finished,
however good the coverage looks.
