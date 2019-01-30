---
description: >-
  This page shall describe what is interface for and how they can be used in
  Keli.
---

# Interface

## Purpose of interface

Interfaces exist to reduce code duplication. 

Suppose we have a function that will return the largest integer from a list of integer.

```bash
="Definition"
(xs int.list).max | int = 
    xs.foldl (prev next | next.>prev. 
                true? next 
                false? prev)

="Usage"
=[5 4 3 2 1].max
```

Then, assume that you need to create another similar function that will return the longest string from a list of strings.

```bash
="Definition"
(xs str.list).max | str =
    xs.foldl (prev next | next.length.>(prev.length).
                true? next 
                false? prev)
="Usage"
=["duck" "boy" "hi"].max
```

If you notice careful enough, you can see that both functions are really similar, the only difference is the comparison part:

| Function | Comparison |
| :--- | :--- |
| max of integers | `next.>prev` |
| max of strings | `next.length.>(prev.length)` |

Actually there are two ways to refactor this code to reduce code duplication:

* Using lambdas
* Using interface

### Using lambdas

We could rewrite the `max` function as `maxBy` using generics and lambdas:

```bash
{a type}
(xs a.list).maxBy comparer:(a.pair a.to a) | a =
    xs.foldl comparer
    
="Usage"
=[5 4 3 2 1].maxBy(x y | x.>y.true? x false? y)
=["a" "cd" "def"].maxBy
    (x y | x.length.>(y.length).true? x false? y)
```

However, as we can see, it is a bit clumsy to use the newly defined functions. 

So, to further improve readability, we should use interface.

### Using interface

First, we have to define the interface, let's call it `comparable`.

```text
comparable = interface;
```

From the previous code, we can see that both `max` functions are using the same operator, which is `>` . 

To prevent collision, we will use the identifier `greaterThan`, instead of `>`.

So, we shall say that if some type is `comparable`, it needs to define a `greaterThan` function. We can express that in Keli as:

```text
{a comparable}
(x a).greaterThan(y a) | bool = toBeDefined;
```

Then, we could create a generic function `max` using the `comparable` interface:

```text
{a comparable}
(xs a.list).max | a =
    xs.foldl(x y | x.> y.true? x false? y);
```

Let us try using the function:

```bash
=[5 4 3 2 1].max
="Error: `int` is not `comparable`"
```

We would get an error an shown above, because we have not implemented the required function for `comparable` , which is `greaterThan`.

To implement it is quite easy, you just have to defined the `greaterThan` function for  `int`.

```text
(x int).greaterThan(y int) | bool = x.> y
```

After that, when we try to run the same function again, we would get compile error anymore:

```bash
=[5 4 3 2 1].max
="No problem"
```

## Interface conjunctions

You can combine two interfaces together using the `and` function.

For example,

```text
foo = interface
bar = interface
foobar = foo.and bar
```

