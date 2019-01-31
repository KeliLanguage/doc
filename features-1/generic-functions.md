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

```hs
{A Type} {B Type}
```

The code above is saying `A` and `B` are type parameters which can bear any types.

Then, we can define the function as follows:

```hs
{A Type} {B Type}
(this A.Pair(B)).swap | B.Pair(A) = this.second.pair(this.first)
```
