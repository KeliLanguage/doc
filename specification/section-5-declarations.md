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

> _constId_ `=` _expr_

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

> `(` _paramId typeAnnotation_ `)` __`.` _funcId_ \[ `|` _returnTypeAnnotation_ \] `=` _expr_

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

> `(` _paramId typeAnnotation_ `)` __`.` { _funcId_ `(` _paramId typeAnnotation_ `)` } \[`|` _returnTypeAnnotation_ \] `=` _expr_

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

> { `{` _typeVarId_ _typeConstraint_ `}` } \( _unifuncDecl_ \| _polyfuncDecl_ \)

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

> _typeAliasId_ `=` [_recordTypeAnnotation_](section-7-built-in-types.md#7-1-4-record)\_\_

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

> `tag` `.` _tagId_

_tagId_ should follows the `camelCase` naming convention. 

Example of carryless tags \(note that the following piece of code is invalid, it's just for demonstration purpose\):

```text
tag.red
tag.green
tag.blue
```

#### 5.4.1.2 Carryful tag

Carryful tag are tags that carry some specific payload. They can be created using the following grammar:

> `tag` `.` `(` [_typeAnnotation_](section-7-built-in-types.md) __`)`

Example of carryful tags:

```text
tag.red(Int)
tag.green(String)
tag.ok(record.value(String))
```

### 5.4.2 Unions

Tag by themselves are not useful unless they are associated with a union. A union can be created using the following grammar:

> _unionId_ `=` _tagDecl_  { `.or` __`(` _tagDecl_ `)` }

_unionId_ should follow the `PascalCase` convention. _tagDecl_ is either a carryless tag or a carryful tag.

For example,

```text
Color = tag.red
    .or(tag.yellow)
    .or(tag.green(Int))
```

_unionId_ can be used as  tag constructor prefix or type annotation.

### 5.4.3 Union name as tag constructor prefix

For example, we can use the identifier `Color` to create carryless tag and carryful tag.

```text
x = Color.red
y = Color.green(99)
```

The type of `x` and `y` are both `Color` .

### 5.4.4 Union name as type annotation 

For example, we can use the identifier `Color` as function parameter type annotation.

```text
(this Int).green | Color = Color.green(this)
```

## 5.5 Type constructor declarations

Type constructors \(a.k.a generic types\) are actually function that takes one or more types and return a new type. In Keli, there are 2 kinds of type constructor, namely record type constructor and tagged union type constructor.

### 5.5.1 Record type constructor

Record type constructor can be declared using the following grammar:

> _typeConstructorId_ `.` { _id_ `(` _typeVarId_  _typeConstraint_ `)` __} `=` [_recordTypeAnnotation_](section-7-built-in-types.md#7-1-4-record)\_\_

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

## 5.6 Interface declarations

### 5.6.1 

### 5.6.2 Non-extendability 



