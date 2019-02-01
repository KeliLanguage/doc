# Multiple dispatch

[Multiple dispatch](https://en.wikipedia.org/wiki/Multiple_dispatch) is a powerful feature that allows multiple functions to share the same identifiers as long as their parameters have different types. 

> You can think of it as multiple people having the same name but wearing different kind of shirts.

For example:

```haskell
="Adding two integers"
(x Int).+ (y Int) | Int = undefined

="Concatenating two strings"
(x String).+ (y String) | String = undefined 

="The following will be valid"
=1.+(2)
="Hello".+("world")

="The following will be invalid"
=1.+("3")
```



