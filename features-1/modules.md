---
description: >-
  This page shall describe how to import/export modules in Keli, and how
  encapsulation can be achieved.
---

# Modules

## Import/export modules

Importing module in Keli is simple, you just have to import the filename, using [Unix pathname](https://www.geeksforgeeks.org/absolute-relative-pathnames-unix/). 

Suppose we have the following file structure.

```text
my-keli-project/
    app.keli
    math.keli
```

```bash
="math.keli"
x:float,square_root | float = undefined

x:float,cube_root | float = undefined
```

To import all the functions from `math.keli` into `app.keli`, we need to call the `module,import` function

```bash
="app.keli"
=module,import "math.keli"

app = console,log(12.34, square_root)
```

## Importing rules

The importing rules is as simple as follows:

> If A imports B, and B imports C, the declarations in C are still hidden from A.

## Encapsulation

From time to time, when we are creating libraries for others to consume, we might want to encapsulate \(hide\) some functions from the users. 

But, by default, every function is exported in Keli. 

We can achieve this behavior by exploiting the [importing rules](modules.md#importing-rules). But that would be crumblesome to do.

So, the way to encapsulate functions are to use naming conventions. For private functions, simply prefix their identifiers with underscore `_` .

For example:

```bash
="drawing.keli"

drawer=drawer

x:drawer,draw_square | shape,io = undefined

x:drawer,draw_circle | shape,io = undefined

x:drawer,_plot coordinate:(float,pair float) | shape,io = undefined
```

In this case, the `_plot` function is private, however, the compiler does not restrict the user from using it at all. Because sometimes, user might want to access some private functions in order to achieve their goal. 

