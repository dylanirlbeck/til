# Pattern matching with the `when` clause

There are times when I've found myself wanting to pattern match against values
defined in another module, only to get a compiler error when attempting to do
so. Let's see an example.

Say I have a file that defines some hex values for colors:

```reason
/* Colors.re */

let mono100 = "FFFFFF";
let mono200 = "D4DFED";
```

It'd be nice to be able to pattern match against `mono100`, `mono200`, etc. in
another file without knowing there actual values. However, the following code
would give a syntax error:

```reason
/* AnotherModule.re */

// ERROR: Syntax error
let hex = switch(someColor) {
  | Colors.mono100 => // Do something...
  | Colors.mono200 => // Do something else...
}
```

What's the solution? Well it turns out we can hack the compiler a bit bymaking
use of the [`when`
clause](https://reasonml.github.io/docs/en/pattern-matching#when-clauses).

```reason
/* AnotherModule.re */

// This works!
let hex = switch (color) {
    | c when c == Colors.mono100 => // Do something...
    | c when c == Colors.mono200 => // Do something else...
    };
```

Happy pattern matching!
