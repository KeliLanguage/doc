---
description: >-
  This page shall describe what is interface for and how they can be used in
  Keli.
---

# Interface

## Purpose of interface

Interfaces exist to reduce code duplication.

Suppose we have a function that will return the largest integer from a list of integer.

```haskell
="Definition"
(this List.of(Int)).max | Int = 
    xs.foldl
      (prev | next | 
        next.moreThan(prev). 
          true?  (next)
          false? (prev))

="Usage"
=[1 2 3 4 5].max
```

Then, assume that you need to create another similar function that will return the longest string from a list of strings.

```haskell
="Definition"
(this List.of(String)).longest | String =
    xs.foldl
      (prev | next | 
        next.length.moreThan(prev.length).
                true? (next)
                false? (prev))
="Usage"
=["duck" "boy" "hi"].longest
```

If you notice careful enough, you can see that both functions are really similar, the only difference is the comparison part:

| Function | Comparison |
| :--- | :--- |
| max of integers | `next.moreThan(prev)` |
| max of strings | `next.length.moreThan(prev.length)` |

Actually there are two ways to refactor this code to reduce code duplication:

* Using lambdas
* Using interface

### Using lambdas

We could rewrite the `max` function as `maxBy` using generics and lambdas:

```haskell
{A Type}
(this List.of(A)).maxBy(comparer A.To(A).To(A)) | A =
    xs.foldl(comparer)

="Usage"
=[5 4 3 2 1].maxBy(x | y | x.moreThan(y).true? (x) false? (y))

=["a" "cd" "def"].maxBy
    (x | y | x.length.moreThan(y.length).true? (x) false? (y))
```

However, as we can see, it is a bit clumsy to use the newly defined functions.

So, to further improve readability, we should use interface.

### Using interface

First, we have to define the interface, let's call it `comparable`.

```text
Comparable = interface
```

From the previous code, we can see that both `max` functions are using the same function, which is `.moreThan` .

To prevent collision, we will use the identifier `greaterThan`, instead of `moreThan`.

So, we shall say that if some type is `Comparable`, it needs to define a `greaterThan` function. We can express that in Keli as:

```haskell
{A Type.subtypeOf(Comparable)}
(this A).greaterThan(that A) | Boolean = toBeDefined
```

Then, we could create a generic function `max` using the `comparable` interface:

```haskell
{A Type.subtypeOf(Comparable)}
(this List.of(A)).max | A =
    xs.foldl(x | y | x.greaterThan(y).true? (x) false? (y))
```

Let us try using the function:

```bash
=[5 4 3 2 1].max
="Error: `Int` is not `Comparable`"
```

We would get an error an shown above, because we have not implemented the required function for `Comparable` , which is `greaterThan`.

To implement it is quite easy, you just have to defined the `greaterThan` function for `Int`.

```haskell
(this Int).greaterThan(that Int) | Boolean = x.moreThan(y)
```

After that, when we try to run the same function again, we would get compile error anymore:

```bash
=[5 4 3 2 1].max
="No problem"
```

## Interface conjunctions

You can combine two interfaces together using the `and` function.

For example,

```haskell
Foo = interface
Bar = interface
FooBar = Foo.and(Bar)
```

