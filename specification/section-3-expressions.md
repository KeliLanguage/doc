# Section 3: Basic Expressions

Since Keli is adapting Smalltalk's syntactical minimalism, there are only 3 kinds of expressions in Keli, namely literal expressions, lambda expressions and function invocation. 

Every expression in Keli can be captured using the following EBNF grammar. However, one should note that the `FuncCall` node might be translated into other expression that bears different semantics, as explained in [Section 4](section-4-magic-expressions.md).

```text
Expr
    = FuncCall
    | LitExpr
    | Lambda
    | '(' Expr ')'

FuncCall
    = Expr '.' FuncCallTail

FuncCallTail
    = FuncId 
    | {FuncId '(' Expr ')'}

LitExpr
    = NumberLit
    | StringLit
    | ConstLit
    | ArrayLit

ArrayLit
    = ('[' ']' | '[' Expr {',' Expr} ']')
    
Lambda
    = Id '|' Expr
    = '.' FuncCallTail
```

## 3.1 Abbreviations

| Abbreviations | Meaning |
| :--- | :--- |
| lit | literal |
| expr | expression |
| func | function |
| id | identifier |
| invo | invocation |
| const | constant |
| unifunc | function that takes one and only one parameter |
| polyfunc | function that takes more than one parameter |

## 3.2 Literal expressions

### 3.2.1 Number literal

#### 3.2.1.1 Integer literal

Integer literal are one or more sequences of digits, as described in [Section 2.](chapter-2-lexical-structure.md)  For example,

```text
12345
```

#### 3.2.1.2 Float literal

Float literal are one or more sequences of digits followed by a period and one or more sequences of digits. For example,

```text
3.141592653
```

### 3.2.2 String literal

String literals are any lexemes enquoted by double quotes. For example,

```java
"Hello world"
```

### 3.2.3 Constant literal

A constant literal is any valid identifier of Keli \(as described in [Section 2.5](chapter-2-lexical-structure.md#2-5-identifiers-and-operators)\). For example, the following are  valid constant literals:

```text
x
point1
www-www
```

### 3.2.4 Array literal

 Array are transpiled directly as native JavaScript array. Also, array needs to be homogeneous, i.e., all the element of an array must be of the same type.

Array type can be created using the following grammar:

\( `[` _expr_ { `,` _expr_ } `]` \| `[]` \)

For example,

```c
[1,2,3,4,5]
[] // empty array
```

## 3.3 Function invocations

Function invocations is the heart of Keli, because almost everything in Keli can be viewed as function invocations. In brief, there are only 2 kinds of function invocations, namely, unifunc invocations and polyfunc invocations.

### 3.3.1 Function identifiers

A function identifier can be any combination of alphabets and also any combinations of operators \(except brackets\). For example, the following are valid function identifiers:

```text
print
+
-
hello?
->
==
```

Also, because of polyfunc invocations, functions in Keli are actually identified by one or more function identifiers. 

### 3.3.2 Precedence rule

The precedence rule is very simple:

1. Evaluate any expressions in the most nested parenthesis first.
2. If no parenthesis is found, evaluate from left to right until the dot operator is encountered.

### 3.3.3 Unifunc invocations

Unifunc invocations are functions invoked with exactly one parameter. The grammar for unifunc invocations are as follows:

> _expr_ `.` _funcId_

```text
Expr '.' FuncId
```

For example, in the following code, `123` is the parameter and `square` is the function identifier. In other words, it means applying `123` to the function `square` .

```text
123.square
```

Due to the precedence rule as described in [Section 3.3.2](section-3-expressions.md#3-3-2-precedence-rule), the following code snippets are equivalent:

```text
123.negate.square
```

```text
(123.negate).square
```

### 3.3.4 Polyfunc invocations

Polyfunc invocations are functions invoked with more than one parameter.

The grammar for polyfunc invocations are as follows:

> _expr_ `.` __{ _funcId_ `(` _expr_ `)` }

For example, the following code means applying `1` and `x` to the function `plus`,

```text
1.plus(x)
```



The following code means applying `"Hello world"` , `"Hello"`  and `"Bye"` to the function named `replace` `with` ,

```java
"Hello world".replace("Hello") with("Bye")
```



As mentioned in [Section 3.3.1](section-3-expressions.md#3-3-1-function-identifiers), since operators are also valid identifiers, the following code is valid, and shall mean applying `myHashMap` , `"key"` and `123`to the function `add` `->` . 

```java
myHashMap.add("key")->(123)
```

Due to such flexibility, one can easily create custom DSL \(domain-specific languages\) using Keli. In fact, Keli itself also abuses this flexibility for compiler-specific commands, such as defining tagged union, foreign function interface etc.

Because of the the [precedence rule](section-3-expressions.md#3-3-2-precedence-rule), one can chain function invocations as such:

```text
myList
    .append(5)
    .reverse
    .put(5) atIndex(0)
    .++(anotherList)
    .select(x | x.isEven)
```

## 3.4 Lambda expressions

Lambda are also known as anonymous functions, which are functions that is nameless. 

Lambda can be created using the following grammar:

> _id_  \[ _typeAnnotation_ \]`|` _expr_

For example, the following code means `x` is the parameter and `x.+(5)` is the lambda body.

```text
x | x.+(5)
```

Lambda with more than one parameters is not supported, since one can easily emulate that using nested lambdas, as such:

```text
x | y | x.+(y)
```

To apply argument to a lambda expression, use the magic function `.apply` as follows:

```text
f = (x | x.square)
```

### 3.4.1 Lambda shorthand

Since most of the time we will only be using a single parameter lambda, we could use lambda shorthand to reduce typings and improve readability. Lambda shorthand can be created using the following grammar:

> `.` \( _funcId_ \| { _funcId_  `(` _expr_ `)` }\)

For example,

| Shorthand | Expansion |
| :--- | :--- |
| `.reverse` | `x | x.reverse` |
| `.+(2)` | `x | x.+(2)` |
| `.replace("Hello") with("Hi")` | `x | x.replace("Hello") with("Hi")` |

Lambda shorthand are useful especially when we are using higher-order functions like `select` .

For example,

```c
[1, 2, 3, 4, 5].select(.*(2)) // [2 4 6 8 10]
```

### 3.4.2 Type inference and multiple dispatch

Most of the time, the type of the parameter of a lambda can be inferred by the compiler, however, in certain situation the compiler might not be able to deduce the parameter type. 

Suppose we have two `negate` functions.

```text
(this Int).negate = undefined
(this String).negate = undefined
```

After, if we create a lambda as follows, we will get a compile error:

```c
f = x | x.negate
// Unable to deduce the type of `x`, because `x` might be `String` or `Int`
```

In such situation, we can provide a type annotation for the parameter to overcome this issue:

```text
f = x Int | x.negate
```

