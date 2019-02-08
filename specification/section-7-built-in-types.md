# Section 7: Type annotations

## 7.0 Purpose of type annotations

The primary purpose of type annotations are for type checking, so that we can prevent a handful of bugs to be introduced into our program albeit it cannot detect logical errors. Moreover, type annotations can also serve as useful documentations.

Since Keli is a minimal language, type annotations are actually basic expressions in disguise. For example, the type annotation `List.of(Int)` is actually a polyfunc invocation.

In Keli, there are 6 places where we can write down type annotations:

1. Function parameters
2. Function return type
3. Record property value type
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

### 7.1.4 Record

Record type annotation can be created using the following grammar:

> `record` `.` { _key_ `(` _typeAnnotation_ `)` }

For example,

```c
record.name(String) age(Int)
```

A valid value for the type annotation above is:

```c
record.name("Keli") age(99)
```

### 7.1.5 Lambda

The type annotation for lambda expression \(or functions\) can be created using the following grammar:

> _inputTypeAnnotation_ `.` `->` `(` _outputTypeAnnotation_ `)`

For example,

| Annotation | Sample value |
| :--- | :--- |
| `Int.->(Int)` | `(x | x.square)` |
| `Int.->(Int).->(Int)` | `(x | y | x.+(y))` |



