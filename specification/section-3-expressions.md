# Section 3: Expressions

Since Keli is adapting Smalltalk's syntactical minimalism, there are only 3 kinds of expressions in Keli, namely literal expressions, lambda expressions and function invocation. 

The grammar for Keli expressions is described below using EBNF:

```text
Expr
    = FuncInvo
    | LitExpr
    | Lambda
    | '(' Expr ')'

FuncInvo
    = Expr '.' (FuncId | {FuncId '(' Expr ')'})

LitExpr
    = NumberLit
    | StringLit
    | ConstLit
    
Lambda
    = Id '|' Expr
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

```text
Expr '.' { FuncId '(' Expr ')' }
```



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

```text
Id '|' Expr 
```

For example, the following code means `x` is the parameter and `x.+(5)` is the lambda body.

```text
x | x.+(5)
```

Lambda with more than one parameters is not supported, since one can easily emulate that using nested lambdas, as such:

```text
x | y | x.+(y)
```



