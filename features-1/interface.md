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
xs:(int,list),max | int = 
    xs,foldl (prev next | next,>prev, 
                if_true next 
                if_false prev)

="Usage"
=[5 4 3 2 1],max
```

Then, assume that you need to create another similar function that will return the longest string from a list of strings.

```bash
="Definition"
xs:(str,list),max | str =
    xs,foldl (prev next | next,length,>(prev,length),
                if_true next 
                if_false prev)
="Usage"
=["duck" "boy" "hi"],max
```

If you notice careful enough, you can see that both functions are really similar, the only difference is the comparison part:

| Function | Comparison |
| :--- | :--- |
| max of integers | `next,>prev` |
| max of strings | `next,length,>(prev,length)` |

Actually there are two ways to refactor this code to reduce code duplication:

* Using lambdas
* Using interface

### Using lambdas

We could rewrite the `max` function as follows using generics and lambdas:

```bash
{a:type}
xs:(a,list),max_by comparer:(a,pair a,to a) | a =
    xs,foldl comparer
    
="Usage"
=[5 4 3 2 1],max_by(x y | x,>y,if_true x if_false y)
=["a" "cd" "def"],max_by
    (x y | x,length,>(y,length),if_true x if_false y)
```

However, as we can see, it is a bit clumsy to use the newly defined functions. 

So, to further improve readability, we should use interface.

### Using interface

First, we have to define the interface, let's call it `comparable`.

```text
comparable = interface
```

From the previous code, we can see that both `max` functions are using the same operator, which is `>` . 

To prevent collision, we will use the identifier `greater_than`, instead of `>`.

So, we shall say that if some type is `comparable`, it needs to define a `greater_than` function. We can express that in Keli as:

```text
{a:comparable}
x:a, greater_than y:a | bool = to_be_defined
```

Then, we could create a generic function `max` using the `comparable` interface:

```text
{a:comparable}
xs:(a,list),max | a =
    xs,foldl(x y | x, > y, if_true x if_false y)
```

Let us try using the function:

```bash
=[5 4 3 2 1],max
="Error: `int` is not `comparable`"
```

We would get an error an shown above, because we have not implemented the required function for `comparable` , which is `greater_than`.

To implement it is quite easy, you just have to defined the `greater_than` function for  `int`.

```text
x:int, greater_than y:int | bool = x, > y
```

After that, when we try to run the same function again, we would get compile error anymore:

```bash
=[5 4 3 2 1],max
="No problem"
```

## Interface extensions

You can extend an interface using the `,extends` function.

For example,

```text
equatable = interface
comparable = interface,extends equatable
```

## Interface conjunctions

You can combine two interfaces together using the `,and` function.

For example,

```text
foo = interface
bar = interface
foobar = foo,and bar
```

