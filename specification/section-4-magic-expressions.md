# Section 4: Magic Expressions

Magic expressions are valid basic expression in disguise but are treated differently by the compiler.

## 4.1 Records

Records can be viewed as list of key-value pairs, where the values can be heterogeneous.

### 4.1.1 Record creation

To create a record, we need to use the following grammar:

> `record` `.` { _propertyId_ `(` _propertyValue_`)` }

_propertyId_ can be any valid [constant identifier](chapter-2-lexical-structure.md#2-5-constant-identifiers), while _propertyValue_ can be any valid expression.

For example, the following is equivalent to JSON `{"name": "Keli", "age": 20}`. In fact, it is actually a [polyfunc invocation](section-3-expressions.md#3-3-4-polyfunc-invocations), where the parameters are `record`, `"Keli"` and `20` while the function name is `name` `age` . 

```haskell
record.name("Keli") age(20)
```

Since Keli supports structural typing, the expression above bears the type `record.name(String) age(Int)` . 

### 4.1.2 Property getter

Property getters are for accessing the value of a record given a key.  Property getters can be invoked via the following grammar:

> _recordExpr_ `.` _propertyId_

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

Property setters is a polymorphic function that can either takes in a new value, or a lambda that can be used for referential update.

Property setters can be invoked via the following grammar:

> _recordExpr_ `.` _propertyId_ `(` _newValue_ \| [_lambda_](section-3-expressions.md#3-4-lambda-expressions) `)`

#### 4.1.3.1 Direct value update

Direct value update refers to the fact that we update the value of a property without referencing its original value.

For example,

```c
foo = record.name("Keli") age(20)
bar = foo.name("Pine") 
= foo // foo remain unchanged
= bar // record.name("Pine") age(20)
```

#### 4.1.3.2 Referential value update

Referential value update means that we want to update the value of a property by applying some functions to its original value.

Consider the following example where `person` wants to decrease its car's price by 10%.

```c
person1 = record.
    name("John")
    car(record.
        brand("Yoyota")
        price(999))

// Using lambda
updatedPerson1 = person1.car(c | c.price(p | p.*(0.9)))

// Using lambda shorthand
updatePerson2 = person1.car(.price(.*(0.9)))
```

#### 4.1.3.3 Function signature clashing

Since property setters are actually [polyfunc invocation](section-3-expressions.md#3-3-4-polyfunc-invocations), we cannot declare polyfunc that takes 2 parameters where its identifier matches one of the property name of the subject \(the first parameter\) and the second parameter have the same type as of the corresponding property setters. 

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

```c
Shape = tags.
    #(circle.radius(Float))
    #(rectangle.height(h) width(w))
    #(empty)
```

Tag matchers are magic functions that can only be invoked on expression that have the type of tagged union, and they can be invoked using the following grammar:

> _tagExpr_ `.` { _carrylessTagBranch_ \| _carryfulTagBranch_ \| _elseBranch_ }

where:

> _carrylessTagBranch =_  `if` __`(` _tagId_  `)` `:` `(` _branchExpr_ `)` __
>
> _carryfulTagBranch_ = `if` __`(` _tagId_ `.` __{  _propertyId_ `(` _constId_ `)` }`)` `:` `(` _branchExpr_ `)` __

> _elseBranch_ = `else` \[ `(` _constId_ `)` \] `:` `(` _branchExpr_ `)`

There are two kinds of tag matchers, namely exhasutive and non-exhaustive. 

### 4.3.1 Exhaustive matching

Exhaustive matching means every possible tag is matched. Consider the following function where the first parameter is type of `Shape`.

```c
(this Shape).area | Float =
    this.
        if(circle.radius(r)):    
            (pi.*(r.^(2)))
        if(rectangle.height(h) width(w)): 
            (h.*(w))
        if(empty): 
            (0.0)
```

Example output :

| Input | Output |
| :--- | :--- |
| `Shape.circle.radius(3)` | `28.2743` |
| `Shape.rectangle.height(3) width(4)` | `12.0` |
| `Shape.empty` | `0.0` |

The indentation presented in the code above is just for formatting purpose, as Keli is not indentation-sensitive. 

### 4.3.2 Non-exhaustive matching

Non-exhaustive matching means not all possible tags are listed, however one of the tag must be an `else:` .

For example,

```c
(this Shape).isEmpty | Boolean =
    this.
        if(empty):  
            (Boolean.true)
        else: 
            (Boolean.false)
```

Sample output:

| Input | Output |
| :--- | :--- |
| `Shape.circle.radius(3)` | `Boolean.false` |
| `Shape.rectangle.height(3) width(4)` | `Boolean.false` |
| `Shape.empty` | `Boolean.true` |

In some cases we might also want to bind the value from `else:` branch, consider the following code that increment the radius of `circle` but not the other shape:

```c
(this Shape).increaseRadius | Shape =
    this.
        if(circle.radius(r)):
            (Shape.circle.radius(r.*(1.5)))
        else(otherShape):
            (otherShape)
```

### 4.3.3 Branch homogeneity

All branches must have the same type as the first branch. Thus, the following code is invalid:

```c
= Shape.circle.radius(12.0).
    if(circle.radius(r)):     
        (123)
    if(rectangle.height(h) width(w)):  
        (Boolean.false) // Error, expected `Int` but got `Boolean`
    if(empty): 
        ("lol") // Error, expected `Int` but got `String`
```

### 4.3.4 Static analysis 

A valid tag matcher must satisfy the following criteria:

1. No duplicated tag
2. Must be exhaustive unless the `else` branch is present.
3. No undefined tag \(in the sense that it is not defined in the corresponding tagged union\). 
4. No more than one `else` branch is present for non-exhaustive matching.



### 4.3.5 Optional bindings

At certain situation, the bindings of a carryful tag might not be fully utilized, in such cases, we can simply not specify those unneeded properties.

For example, suppose we have the following tagged union:

```c
Food = tags.  
    #(burger.
            price(Float) 
            isCheesy(Boolean))
            
    #(coke.
            price(Float) 
            sugarLevel(Float))
```

And a function to check if a `Food` is expensive:

```c
(this Food).isExpensive | Boolean = 
    this.
        if(burger.price(p) isCheesy(i)):
            (p.>(30))
        if(coke.price(p) sugarLevel(s)):
            (p.>(10))
```

In the function above, we can see that bindings `i` and `s` are not used at all, so we actually erase them out to have a less noisy code as follows:

```c
(this Food).isExpensive | Boolean = 
    this.
        if(burger.price(p)):
            (p.>(30))
        if(coke.price(p)):
            (p.>(10))
```

Besides ignoring some properties, we could also ignore all properties, consider the following function that check if a `Food` is drinkable.

```c
(this Food).isDrinkable | Boolean =
    this.
        if(burger):
            (Boolean.true)
        if(coke):
            (Boolean.false)
```

## 4.4 Foreign function interface \(FFI\)

Foreign function interface is used to invoked JavaScript function from Keli. FFI is a kind of expression that can be created using the following grammar:

> `ffi` `.` `javascript` `(` `"` _javascriptCode_ `"` __`)` __

The type of an FFI expression is `undefined` , thus we need to cast it explicitly using the magic function `as` .

Every identifier in Keli can be used in the JavaScript code by prefixing them with a dollar sign. For example, if the identifier is `x` is Keli, then it will be transpiled as `$x` in JavaScript.

Moreover, since the `Boolean` type is not built-in to Keli, but rather defined in the Prelude, we cannot use `true` or `false` in JavaScript, but rather `$Boolean.$true` or `$Boolean.$false` . Similarly, any other tagged union can be returned from the JavaScript code using the following grammar:

> `$` _taggedUnionId_ `.` `$` _tagId_

For example, 

```c
(this Int).>(that Int) = 
    ffi
    .javascript("$this > $that ? $Boolean.$true : $Boolean.$false")
    .as(Boolean)
```

{% hint style="warning" %}
Warning

FFI must be used safely, because annotating foreign functions with incorrect type annotation will allow bugs to slip through type checking and result in run-time error that is hard to debug.
{% endhint %}

