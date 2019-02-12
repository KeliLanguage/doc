# Section 5: Declarations

## 5.0 Evaluated declarations

Evaluated declarations are actually inline expressions, which can be created using the following grammar:

> `=` _expr_

Not only that, the value of the evaluated expressions will also be shown on STDOUT. However, evaluated declarations will only be evaluated if they are written in the [Main module](section-6-modules.md#6-1-the-main-module), except for [module imports](section-6-modules.md#6-4-imports).

For example, if we interpret the following Keli program,we shall see `120` on STDOUT. 

```text
=5.factorial
```

## 5.1 Constant declarations

Constant declarations are useful for defining common constants such as the value of pi, _e_, etc. They can be declared using the following grammar:

> \_\_[_constId_](chapter-2-lexical-structure.md#2-5-constant-identifiers) `=` _expr_

For example,

```text
pi = 3.141592653
```

Once a constant is declared, it can be accessible anywhere within the same module \(or those which imported it\). 

Constants cannot be reassign new values, thus the following is erroneous:

```c
pi = 3.142
pi = 3.142
// Error: Duplicated constant identifier `pi`
```

## 5.2 Function declarations

There are two kinds of function in Keli, namely _unifunc_ and _polyfunc_. 

### 5.2.1 Unifunc declarations

Unifunc can be created using the following grammar:

> _unifuncSignature_ `=`[_expr_](section-3-expressions.md)\_\_

where: 

> _unifuncSignature_ = `(` __[_paramId_](chapter-2-lexical-structure.md#2-5-constant-identifiers) __[_typeAnnotation_](section-7-built-in-types.md) __`)` __`.` [_funcId_](chapter-2-lexical-structure.md#2-6-function-identifiers) \[ `|` [_returnTypeAnnotation_](section-7-built-in-types.md) \]

For example,

```haskell
(this Int).square | Int = this.*(this)
```

In the code above, the first `this` is the parameter for the function `square` , where `this` should be type of `Int`. The body of this function is `this.*(this)` , while the return type of this function is also `Int`.

Return type annotation for functions are optional \(since it can be inferred\), so the `square` function can be rewritten as :

```haskell
(this Int).square = this.*(this)
```

### 5.2.2 Polyfunc declarations

Polyfunc can be created using the following grammar:

> _polyfuncSignature_ `=` [_expr_](section-3-expressions.md)\_\_

where

> _polyfuncSignature_ =`(` [_paramId_](chapter-2-lexical-structure.md#2-5-constant-identifiers) __[_typeAnnotation_](section-7-built-in-types.md) __`)` __`.` { [_funcId_](chapter-2-lexical-structure.md#2-6-function-identifiers) __`(` [_paramId_](chapter-2-lexical-structure.md#2-5-constant-identifiers) __[_typeAnnotation_](section-7-built-in-types.md) __`)` } \[`|` [_returnTypeAnnotation_](section-7-built-in-types.md) \] `=`

For examples:

```c
// 2-param polyfunc
(this Int).plus(that Int) = undefined

// 3-param polyfunc
(this String).replace(sub String) with(new String) = undefined

// 4-param polyfunc
(this List.of(Int)).replace(startIndex Int) to(endIndex Int) with(new Int) = undefined
```

Same as unifunc, the return type annotation of polyfunc declaration is also optional.

### 5.2.3 Multiple dispatch

Keli supports [multiple dispatch](https://en.wikipedia.org/wiki/Multiple_dispatch) \(a.k.a function overloading\), thus it is possible to declare multiple function that have the same set of identifiers as long as their parameters type does not fully match.

For example, the following Keli code is valid:

```c
(x Int).+(y Int) = undefined
(x Float).+(y Float) = undefined
(x Int).+(y Float) = undefined
```

Moreover, functions with overlapping identifiers are also permitted:

```c
(x String).replace(y String) = undefined
(x String).replace(y String) with(z String) = undefined
```

The following code is invalid, although the parameters name are different:

```c
(x Int).plus(y Int) = undefined
(a Int).plus(b Int) = undefined
// Error: Duplicated functions
```

Also, Keli does not supports multiple dispatch based on return types, so the following code is invalid despite different return types:

```c
(x Int).plus(y Int) | String = undefined
(x Int).plus(y Int) | Int = undefined
// Error: Duplicated functions
```

### 5.2.4 Generic functions

Generic functions are functions whose parameters type are generic. We can declare generic functions using the following grammar:

> { `{` [_typeVarId_](section-5-declarations.md#5-1-constant-declarations) [_typeConstraint_](section-7-built-in-types.md#7-2-type-constraint-annotation) `}` } \( [_unifuncDecl_](section-5-declarations.md#5-2-1-unifunc-declarations) __\| [_polyfuncDecl_](section-5-declarations.md#5-2-2-polyfunc-declarations) \)

_typeVarId_ is any valid identifiers, while _typeConstraint_ is any valid constraint expressions. 

For example, the identity function can be defined as such:

```text
{T Any}
(this T).identity | T = this
```

In the code above, `T` is the type variable identifier, while `Any` is the constraint on `T` , `Any` also means no constraint. 

The type variable `T` is inferred using some sort of Hindley-Milner type inference system.

For example,

```c
x = 123.identity // the type of `x` is inferred as `Int`
y = "Hello".identity // the type of `y` is inferred as `String`
```

Generic function by itself is not too useful unless incorporated with generic types such as generic records or generic tagged union.

## 5.3 Record type alias

Record type alias \(a.k.a struct types\) can be created using the following grammar:

> \_\_[_typeAliasId_](section-5-declarations.md#5-1-constant-declarations) `=` [_recordTypeAnnotation_](section-7-built-in-types.md#7-1-4-record)\_\_

For example,

```haskell
People = record.name(String) age(Int)
```

Record type alias can be used as type annotations, for example,

```haskell
(this People).isOld = this.age.>(50)
```

Moreover, it can also be used as record constructor, as follows:

```c
me = People.name("Keli") age(20)
```

## 5.4 Tagged unions declaration

Tagged unions \(a.k.a discriminated unions OR sum types\) is consists of one or more tags connected together by the `.or` function.

### 5.4.1 Tag 

There are two kinds of tag, namely carryless tag and carryful tag.

#### 5.4.1.1 Carryless Tag

Carryless tag are tags that does not carry any payload with them \(like enums in C or Java\).  They can be created using the following grammar:

> _tagId_

_tagId_ are [constant identifiers](chapter-2-lexical-structure.md#2-5-constant-identifiers) that follows the the `camelCase` naming convention.  

Example of carryless tags \(note that the following piece of code is invalid, it's just for demonstration purpose\):

```text
circle
square
rectangle
```

#### 5.4.1.2 Carryful tag

Carryful tag are tags that carry some specific payload. They can be created using the following grammar:

> _tagId_ `.` { _id_ `(` _typeAnnotation_ `)` }

Example of carryful tags:

```text
circle.radius(Float)
square.side(Float)
rectangle.height(Float) width(Float)
```

### 5.4.2 Unions

Tag by themselves are not useful unless they are associated with a union. A union can be created using the following grammar:

> \_\_[_unionId_](section-5-declarations.md#5-1-constant-declarations) __`=` __`tags` `.`  __{ `#` __`(` [_tagDecl_](section-5-declarations.md#5-4-1-tag) __`)` }

_unionId_ should follow the `PascalCase` convention. _tagDecl_ is either a carryless tag or a carryful tag.

For example,

```haskell
Shape = tags.
    #(circle.radius(Float))
    #(square.side(Float))
    #(rectangle.height(Float) width(Float))
    #(none)
```

_unionId_ can be used as  tag constructor prefix or type annotation.

### 5.4.3 Union name as tag constructor prefix

For example, we can use the identifier `Shape` to create carryless tag and carryful tag.

```text
x = Shape.none
y = Shape.circle.radius(3.2)
```

The type of `x` and `y` are both `Shape` .

### 5.4.4 Union name as type annotation 

For example, we can use the identifier `Color` as function parameter type annotation.

```text
(this Shape).area | Float = undefined
```

## 5.5 Type constructor declarations

Type constructors \(a.k.a generic types\) are actually function that takes one or more types and return a new type. In Keli, there are 2 kinds of type constructor, namely record type constructor and tagged union type constructor.

### 5.5.1 Record type constructor

Record type constructor can be declared using the following grammar:

> \_\_[_typeConstructorId_](chapter-2-lexical-structure.md#2-5-constant-identifiers) `.` { [_id_](chapter-2-lexical-structure.md#2-5-constant-identifiers) `(` [_typeVarId_](chapter-2-lexical-structure.md#2-5-constant-identifiers)  [_typeConstraint_](section-7-built-in-types.md#7-2-type-constraint-annotation) __`)` __} `=` [_recordTypeAnnotation_](section-7-built-in-types.md#7-1-4-record)\_\_

For example, we can encode the tuple type as record type constructor as follows:

```text
Tuple.fst(A Any) snd(B Any) = record.fst(A) snd(B)
```

In the code above, `Tuple` is the type constructor identifier, and it serves two purpose:

1. For constructing new `Tuple`
2. To be used as type annotation.

To construct a new `Tuple` :

```c
myTuple = Tuple.fst("Hello") snd(123)
```

`myTuple` will be inferred to have the type `Tuple.fst(String) snd(Int)` .

To used `Tuple` as type annotation:

```c
{A Any} {B Any}
(this Tuple.fst(A) snd(B)).swap | Tuple.fst(A) snd(B) = 
    Tuple.fst(this.snd) snd(this.fst)
```



### 5.5.2 Tagged union type constructor

Tagged union type constructor \(a.k.a generic tagged union can be constructed using the following grammar:

> \_\_[_constId_](chapter-2-lexical-structure.md#2-5-constant-identifiers) `.` { [_constId_](chapter-2-lexical-structure.md#2-5-constant-identifiers) `(` [_typeVarId_](chapter-2-lexical-structure.md#2-5-constant-identifiers)  [_typeConstraint_](section-7-built-in-types.md#7-2-type-constraint-annotation) __`)` __} `=` [_tagDecl_](section-5-declarations.md#5-4-1-tag)  __{ `.or` __`(` [_tagDecl_](section-5-declarations.md#5-4-1-tag) __`)` }

For example, singly linked list can be defined as such:

```haskell
List.of(A Type) = tags.
    #(nil)
    #(cons.current (A) next(List.of(A))
```

The identifier `List` can be used as:

1. Tag constructor prefix
2.  Type annotation

Using `List` as tag constructor prefix:

```haskell
x = List.nil 
y = List.cons.current(1) next(List.nil)
```

The type of `x` is `List.of(A)` where the type of `y` is `List.of(Int)`. Due to the type inference, the following expression is invalid:

```c
= List.cons.
    current(1) 
    next   (List.cons.
        current("2") // <-- Error: Expected `Int` but got `String`
        next   (List.nil))
```



Using `List` as type annotation:

```text
{A Type}
(this List.of(A)).length | Int = undefined
```

## 5.6 Interface declarations

Interface are a kind of type constraint that allow user to define a set of functions on a specific type. 

To define an interface in Keli, there are 3 steps required:

1. Define the name of the interface.
2. Define a set of functions required by the interface. 
3. Define a set of default functions for the interface.

### 5.6.1 Defining interface

Interface can be defined using the following grammar:

> \_\_[_interfaceId_](chapter-2-lexical-structure.md#2-5-constant-identifiers) __`=` __`interface`

Note that interface identifier should follow `PascalCase` convention.

For example,

```csharp
Comparable = interface
```

### 5.6.2 Defining required functions

Required function can be defined using the following grammar:

> { `{` _typeVarId  interfaceId_ `}` _funcSignature_ `=` __`required` __}

where _funcSignature_ is either a [_unifuncSignature_](section-5-declarations.md#5-2-1-unifunc-declarations) __or [_polyfuncSignature_](section-5-declarations.md#5-2-2-polyfunc-declarations)_._ 

{% hint style="warning" %}
Alert

To defined required function, the return type annotation cannot be omitted like usual functions do.
{% endhint %}

For example, the code below is saying that if a data type is `Comparable` , then it must have the `==` function and `>` function defined.

```text
{A Comparable}
(this A).==(that A) | Boolean = required

{A Comparable}
(this A).>(that A) | Boolean = required
```

{% hint style="info" %}
Note

The set of required functions can only be defined within the module where the interface name is defined.
{% endhint %}

### 5.6.3 Defining default functions

Default functions are functions that is bonded to the specified interface but does not require user to implement them.

Default functions can be declared using the following grammar:

> { `{` [_typeVarId_](chapter-2-lexical-structure.md#2-5-constant-identifiers)  __[_interfaceId_](chapter-2-lexical-structure.md#2-5-constant-identifiers) `}` _funcSignature_ `=` __`default.as` __`(` _expr_ `)`}

Note that unlike [required functions](section-5-declarations.md#5-6-2-defining-set-of-required-functions), the return type annotation of _funcSignature_ can be omitted.

For example, the code below is saying that if a type `A` is `Equatable` , it will have the default functions `!=` and `>=` .

```text
{A Comparable}
(this A).!=(that A) | Boolean = default.as(this.==(that).not)

{A Comparable}
(this A).>=(that A) | Boolean = default.as(this.>(that).or(this.==(that)))
```

Also, default functions can be declared outside of the module where the corresponding interface is defined.

### 5.6.4 Interface usage

Unlike object-oriented languages, where interface identifiers can be used as type annotation, interface identifier can only be used as [type constraint annotations](section-7-built-in-types.md#7-2-type-constraint-annotation). Thus, they can only appear in [generic functions](section-5-declarations.md#5-2-4-generic-functions) or [generic types](section-5-declarations.md#5-5-type-constructor-declarations).

### 5.6.4 Interface implementation

No special construct is needed to implement an interface, we just need to declare the required function by the interface for the specified data type.

