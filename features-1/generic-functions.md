---
description: >-
  This page shall describe the purpose of generic functions and its usage in
  Keli.
---

# Generic functions

## Purpose

Generics are useful for reducing code duplication. 

## Usage

Suppose we want to define a generic `swap` function. 

To define a generic function, we have to first declare the generic parameters, enclosed by curly brackets.

```text
{a type} {b type}
```

The code above is saying `a` and `b` shall have the type of `type`. Basically it means any type.

Then, we can define the function as follows:

```text
{a type} {b type}
(x (a.pair b)).swap | (b.pair a) = b.pair a
```

{% hint style="info" %}
Hint

The first two usage of `pair` and the last usage of `pair` are different.

The first and second `pair` are used as type constructors, while the last `pair` is used as value constructor.

This can happens in Keli as she supports [multiple dispatch](multiple-dispatch.md).
{% endhint %}



