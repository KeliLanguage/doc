# Section 7: Type annotations

## 7.0 Purpose of type annotations

The primary purpose of type annotations are for type checking, so that we can prevent a handful of bugs to be introduced into our program albeit it cannot detect logical errors. Moreover, type annotations can also serve as useful documentations.

Since Keli is a minimal language, type annotations are actually basic expressions in disguise. For example, the type annotation `List.of(Int)` is actually a polyfunc invocation.

In Keli, there are 6 places where we can write down type annotations:

1. Function parameters
2. Function return type
3. Object property value type
4. Carryful tag carry type
5. Lambda parameter
6. Type casting

Although a type annotation can be consist of any valid identifiers, the conventions in Keli dictates all type annotations to be written in `PascalCase` except for special circumstances.

## 7.1 Built-in types

Built-in types are types that can't or isn't encoded in Keli. For example, integers can actually be encoded as [Peano numbers](https://wiki.haskell.org/Peano_numbers) using tagged unions, but due to performance issues we chose to support integer type directly.

### 7.1.1 Integer

| Annotation | Sample value |
| :--- | :--- |
| `Int` | `123` |

### 7.1.2 Float

| Annotation | Sample value |
| :--- | :--- |
| `Float` | `3.142` |

### 7.1.3 String

| Annotation | Sample value |
| :--- | :--- |
| `String` | `"Hello world"` |

### 7.1.4 Object

Object type annotation can be created using the following grammar:

> `object` `.` { _key_ `(` _typeAnnotation_ `)` }

For example,

```c
object.name(String) age(Int)
```

A valid value for the type annotation above is:

```c
object.name("Keli") age(99)
```

### 7.1.5 Array

Array type annotation can be created using the following grammar:

> `Array` `.` `of` `(` _typeAnnotation_ `)` \_\_

For example,

```c
Array.of(Int)
```

A valid value of the type annotation above is:

```c
[1,2,3,4,5]
```

### 7.1.6 Lambda

The type annotation for lambda expression \(or functions\) can be created using the following grammar:

> `Function` `.` ****`in` _\__`(`_\_typeAnnotation_ `)` `out` `(` _typeAnnotation_ `)`

For example,

| Annotation | Sample value |  |  |
| :--- | :--- | :--- | :--- |
| `Function.in(Int) out(Int)` | \`\(x | x.square\)\` |  |
| `Function.in(Int) out(Function.in(Int) out(Int))` | \`\(x | y | x.+\(y\)\)\` |

## 7.2 Type constraint annotation

Type constraint annotation are used in [generic functions](section-5-declarations.md#5-2-4-generic-functions) and [generic types](section-5-declarations.md#5-5-type-constructor-declarations). Its purpose is to limit the kind of types that can be passed into specific generic functions and generic types. In Keli, there are 3 kinds of type constraint, namely no constraint, interface constraint or row type constraint.

### 7.2.1 No constraint

To specify no constraint, we can use the `Any` identifier. For example,

```text
{T Any}
(this T).identity | T = this
```

### 7.2.2 Interface constraint

Refer [Section 5](section-5-declarations.md#5-6-interface-declarations).

### 7.2.3 Row type constraint

Row type constraint are used for achieving [row polymorphism](https://en.wikipedia.org/wiki/Row_polymorphism). Syntactically, a row type constraint annotation is no different than a [object type annotation](section-7-built-in-types.md#7-1-4-object), they only differs on the place where they are written.

For example, we can declare a generic function that takes any objects with the property `age` .

```c
{T Type.extends(object.age(Int))}
(this T).isOld | Boolean = this.age.>=(50)
```

Then, we can pass in object of any shape, as long as it has the property `age`.

```c
= object.name("Keli") age(50).isOld // No error
= object.age(20).isOld // No error
= object.job("Programmer") age(40) // No error
= object.name("Pine").isOld // Error, missing property `age(Int)`
```

