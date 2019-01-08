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

## Tuple

Unlike list, tuple can holds heterogeneous\(different type\) elements. To create a tuple, we need to use the .`pair` function.

```bash
="tuple creation"
x = "age".pair 10

="tuple access"
property = x.first
value    = x.second

="multiple length tuple"
="tuple1 is equivalent to tuple2"
tuple1 = 1.pair 2.pair 3.pair 4
tuple2 = ((1.pair 2).pair 3).pair 4)

="To access the first element in tuple"
=tuple1.first.first.first
```

## Lambdas

Because Keli supports multiple dispatch, every lambda must be annotated with types.

```bash
="Single parameter lambda"
="plusTwoA and plusTwoB are equivalent"
plusTwoA: int.to int = x|x.+2

plusTwoB = (x|x.+2):(int.to int)

="Applying parameters to lambda"
=plusTwoA.apply 5

="Multiple parameter lambda"
plus: int.pair int.to int = x y | x.+ y

="Application"
=plus.apply(2.pair 4)
```

## Annotating expressions

You may annotate an expression with types with the following format:

```text
<expression> : <type>
```

For example:

```text
x = (1.+2):int
```



