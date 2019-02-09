# Section 4: Magic Expressions

Magic expressions are valid basic expression in disguise but are treated differently by the compiler.

## 4.1 Records

Records can be viewed as list of key-value pairs, where the values can be heterogeneous.

### 4.1.1 Record creation

To create a record, we need to use the following grammar:

> `record` `.` { _key_ `(` _value_`)` }

For example, the following is equivalent to JSON `{"name": "Keli", "age": 20}`. In fact, it is actually a [polyfunc invocation](section-3-expressions.md#3-3-4-polyfunc-invocations), where the parameters are `record`, `"Keli"` and `20` while the function name is `name` `age` . 

```haskell
record.name("Keli") age(20)
```

Since Keli supports structural typing, the expression above bears the type `record.name(String) age(Int)` . 

### 4.1.2 Property getter

Property getters are for accessing the value of a record given a key.  Property getters can be invoked via the following grammar:

> _recordExpr_ `.` _propertyName_

 For example,

```c
me = record.name("Keli") age(20)
x = me.name // "Keli"
y = me.age  // 20
```

Because property getters are actually [unifunc invocation](section-3-expressions.md#3-3-3-unifunc-invocations), we cannot declare unifunc which takes the name of any property the given record. For example, the following introduces a compile error:

```c
(this record.name(String) age(Int)).name = undefined 
// Error: function identifier `name` clashes with property getter `name` 
```

### 4.1.3 Property setter

Property setters are for replacing value of a record given a key. Since Keli does not allow implicit mutations, each invocation of a property setter will return a new copy of the specified record.

Property setters can be invoked via the following grammar:

> _recordExpr_ `.` _propertyName_ `(` _newValue_ `)`

For example,

```c
foo = record.name("Keli") age(20)
bar = foo.name("Pine") 
= foo // foo remain unchanged
= bar // record.name("Pine") age(20)
```

Similarly, since property setters are actually [polyfunc invocation](section-3-expressions.md#3-3-4-polyfunc-invocations), we cannot declare polyfunc that takes 2 parameters where its identifier matches one of the property name of the subject \(the first parameter\) and the second parameter have the same type as of the corresponding property setters. 

```c
(this record.name(String) age(Int)).name(newName String) = undefined 
// Error: function identifier `name` clashes with property setter `name`
```

However, since Keli supports multiple dispatch, it is possible to define a polyfunc that takes 2 parameters where its identifier matches one of the property name of the subject \(the first parameter\) as long as the second parameter does NOT have the same type as of the corresponding property setters.

That is to say, the following function declaration is valid, because the second parameter type is `Int` not `String` . 

```c
(this record.name(String) age(Int)).name(value Int) = undefined 
```

### 4.1.4 Aliased constructor

Refer [Section 5.5.1](section-5-declarations.md#5-5-1-record-type-constructor).

## 4.2 Tag constructors

### 4.2.1 Carryless tag constructor

Refer [Section 5.4.1.1](section-5-declarations.md#5-4-1-1-carryless-tag).

### 4.2.2 Carryful tag constructor

Refer [Section 5.4.1.2](section-5-declarations.md#5-4-1-2-carryful-tag).

## 4.3 Tag matchers

Tag matchers \(a.k.a [case expression](https://en.wikibooks.org/wiki/Haskell/Control_structures)\) is the heart of control structure in Keli, as Keli does not provide any if-else structure. 

Before we use tag matchers, we must first defined a [tagged union](section-5-declarations.md#5-4-tagged-unions-declaration). For the sake of demonstration, we will use the following tagged union `Color` for further explanation.

```text
Color = tag.red
    .or(tag.yellow)
    .or(tag.green(Int))
```

Tag matchers are magic functions that can only be invoked on expression that have the type of tagged union, and they can be invoked using the following grammar:

> _tagExpr_ `.` { _carrylessTagBranch_ \| _carryfulTagBranch_ }

where:

> _carrylessTagBranch =_ { _tagId_  `?` `(` _branchExpr_ `)` __}
>
> _carryfulTagBranch_ = { _tagId_  `(` _constId_ `)` `?` `(` _branchExpr_ `)` __}

There are two kinds of tag matchers, namely exhasutive and non-exhaustive. 

### 4.3.1 Exhaustive matching

Exhaustive matching means every possible tag is matched. Consider the following function where the first parameter is type of `Color`.

```c
(this Color).toString | String =
    this.
        red?      ("Stop")
        yellow?   ("Slow down")
        green(x)? ("Go ".++(x.toString))
```

Example output :

| Input | Output |
| :--- | :--- |
| `Color.red.toString` | `"Stop"` |
| `Color.yellow.toString` | `"Slow down"` |
| `Color.green(5).toString` | `"Go 5"` |

Note that the identifier `x` is binded with the value `5` . Also, the indentation presented in the code above is just for formatting purpose, as Keli is not indentation-sensitive. 

### 4.3.2 Non-exhaustive matching

### 4.3.3 Branch homogeneity

All branches must have the same type as the first branch.  

### 4.3.4 Static analysis 

A valid tag matcher must satisfy the following criteria:

1. No duplicated tag
2. Must be exhaustive unless the `else?` branch is present.
3. No undefined tag \(in the sense that it is not defined in the corresponding tagged union\). 

## 4.4 Foreign function interface

