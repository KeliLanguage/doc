# Expressions

## Strings

Strings are enclosed by double quote:

```bash
="Hello world"
```

Multi-line string can be declared using triple-quotes

```python
="""
This is useful for defining JSON 
or other stuff that contain "quotation marks"
"""
```

## Numbers

```bash
="int"
= 123

="float"
= 12.3

="negative numbers"
= -123
= -12.3

="spaces is not allowed"
= - 123
= "invalid"
```

## List

List are useful for holding a bunch of homogeneous\(same type\) elements. 

```bash
=[1 2 3 4]
=[(1.+2) (3.+4) (5.+6)]
=["hello" "world"]
=[[1] [2]]
```

## 

## Lambdas

Because Keli supports multiple dispatch, every lambda must be annotated with types.

```bash
="Single parameter lambda"
plusTwo = (x|x.+2).as(int.to(int))

="Applying parameters to lambda"
=plusTwo.apply(5)

="Multiple parameter lambda (in fact it is nested lambda)"
plus = (x | y | x.+(y)).as(int.to(int).to(int))

="Application"
=plus.apply(2).apply(4)
```

### Lambdas shorthand

| Shorthand | Equivalent translation |
| :--- | :--- |
| `(.+(2))` | `x | x.+(2)` |
| `(.reverse)` | `x | x.reverse` |
| `(.^)` | `x y | x.^(y)` |

{% hint style="info" %}
Note

When using lambdas shorthand, you must always enclosed them using parenthesis.
{% endhint %}

## Annotating expressions

You may annotate an expression with types with the magic function `.as`

For example:

```text
x = (1.+(2)).as(int)
```

This is necessary when dealing with external data, one example is when we are using [Foreign Functions Interface](ffi-javascript.md).

