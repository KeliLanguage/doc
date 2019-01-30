---
description: This page shall describe what is multiple dispatch and how is it useful.
---

# Multiple dispatch

[Multiple dispatch](https://en.wikipedia.org/wiki/Multiple_dispatch) is a powerful feature that allows multiple functions to share the same identifiers as long as their parameters have different types. 

> You can think of it as multiple people having the same name but wearing different kind of shirts.

For example:

```bash
="Adding two integers"
(x int).+ (y int) | int = undefined

="Concatenating two strings"
(x str).+ (y str) | str = undefined 

="The following will be valid"
=1.+2
="Hello".+"world"

="The following will be invalid"
=1.+"3"
```



