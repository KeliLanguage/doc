# Section 6: Modules

As mentioned in [Section 1](chapter-1-introduction.md), a Keli program is actually a set of modules. In fact, each module actually corresponds to one Keli file. 

In the following section, the term _importer_ means the file that is importing other files, while _importee_ means the file that is being imported. In Keli, a source file can be both _importer_ and _importee_ and the same time.

## 6.1 Entry Point

Unlike languages like Haskell, C or Java, there are no main module required for a Keli program. Keli follows the approaches of languages like Python and JavaScript, where the file being interpreted is the entry point of the program. 

## 6.2 Module naming convention

The Keli compiler shall warn the user if the module naming does not follows the `PascalCase` convention.

## 6.3 Encapsulation

By default, all declarations in a module are accessible by any other modules. To achieve encapsulation, Keli adopts a rather tolerant encapsulation mechanism, where the compiler would not trigger any compile error, but warnings instead. 

To make a declaration invisible to other module, prefix the declaration with an underscore symbol. For example,

```c
_pi = 3.142 // invisible to other module

(this Number)._square = undefined // this is invisible to other module

_Colors = tags. // this is invisible to other module
    #(red)
    #(blue)
```

However, this does not means that the declarations above cannot be used by other modules, it will simply trigger compiler warning if the compiler found that other modules are using them.

## 6.4 Import syntax

### 6.4.1 Raw paths

To import a module, we should follow the grammar below:

> `module` `.` `import` `(` _filePath_ `)`

where _filePath_ is any valid [Unix file paths](https://www.geeksforgeeks.org/absolute-relative-pathnames-unix/), be it relative path or absolute path.

Examples as follows:

```c
=module.import("Math.keli")
=module.import("./src/Server.keli")
```

Note that _filePath_ must end with `.keli` , if not the compiler will raise an error, even though _filePath_ points to a valid Keli source file.

### 6.4.2 Aliased paths

In larger projects, using raw paths might be cumbersome, especially when refactoring the folder structures. In such situation, we can use aliased paths, so that the imports can be based on the project root. 

Path aliases must be prefixed with the dollar sign. 

Path aliases can be defined via the compiler command line arguments or by telling the compiler to load a config file.

Path aliases are defined based on the entry point of a program. 

Example of using aliased paths:

```c
=module.import("$root/Math")
```



## 6.5 Scoping rule 

Suppose we have the following folder structure:

```text
- myapp
    - Main.keli
    - Src
        - Math.keli
        - Misc
            - Shape.keli
```

Suppose the `Main` module imports  `Shape` module:

{% code-tabs %}
{% code-tabs-item title="Main.keli" %}
```c
=module.import("Src/Misc/Shape")
```
{% endcode-tabs-item %}
{% endcode-tabs %}

And `Shape` module imports `Math` :

{% code-tabs %}
{% code-tabs-item title="Shape.keli" %}
```c
=module.import("../Math.keli")
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Then, the following outcome should be observed:

* All declarations in `Shape` will be visible to `Main` except private declarations. 
* All declarations in `Math` will be visible to `Shape` .
* However, no declarations in `Math` will be visible to `Main` , although `Main` imported `Shape` which imported `Math`.

In a nutshell, the scoping rule are as follows:

> Importee will only be visible to its corresponding direct importer. In another words, the importees of importee will be not be visible to the importer.

## 6.6 Import conflicts

This section shall define what is considered import conflicts in Keli, how the compiler should behave in such cases. The term _identical_ will be defined first before defining what is a _conflict_. 

### 6.6.1 Identical declarations

This section shall define how [declarations](section-5-declarations.md) are considered _identical_ in Keli. 

#### 6.6.1.1 Mono-nominal declarations

Mono-nominal declarations means any declarations that can be identified by using only a single string token. In Keli, any non-function declaration are considered mono-nominal. 

Two mono-nominal declarations are considered identical if their identifier \(which appears on the left of the assignment operator\) are lexically equal.

For example, all of the following mono-nominal declarations are considered identical:

```c
Foo = 123
Foo = object.name(String) age(Int)
Foo = tags.#(bar) #(baz)
Foo.of(A Type) = tags.#(spam.value(A))
```

#### 

#### 6.6.1.2 Poly-nominal declarations

Poly-nominal declarations are any declarations that cannot be identified by using only a single string token. In Keli, all function declarations are considered poly-nominal, this due to:

* Multiple dispatch, it means two or more functions can bear the same identifiers without introducing compile error.
* Function identifiers can be spitted into different parts,  like the message syntax in Smalltalk.

Two function declarations, say X and Y, are considered identical if all of the following criteria is observed:

1. The arguments length of X is equals to the arguments length of Y.
2. The identifiers of X have the same length with the identifiers of Y \(implementation does not need to include this checking, because criterion 1 implies this criterion.
3. The first identifier of X is lexcially equivalent to the first identifier of Y, and so on and so forth for the rest of the identifiers.
4. The type annotation of the first argument of X is identical to the type annotation of the first argument of Y, and so on and so forth for the rest of the arguments.

The following code snippets demonstrates identical function declarations.

```c
(x Int).plus(y Int) = undefined
(this Int).plus(that Int) = undefined
```

```csharp
(this String).replace(old String) with(new String) = undefined
(x String).replace(y String) with(z String) = undefined
```

The following code snippets demonstrates non-identical function declarations, with the reason commented on top of each snippet.

```c
// Different argument length
(this Int).plus = undefined
(this Int).plus(that Int) = undefined
```

```c
// Different identifiers
(this Int).foo = undefined
(this Int).bar
```

```c
// Different argument type annotation
(x Int).plus(y Int) = undefined
(x Int).plus(y Float) = undefined
```

```c
// Different identifiers (although would be identical after sorting)
(x Int).foo(y Int) bar(z Int) = undefined
(x Int).bar(y Int) foo(z Int) = undefined
```

### 6.6.2 Identical type annotations

In Keli, there are there kinds of type annotation \(TA\), namely, simple TA, compound TA and bounded type variables.

#### 6.6.2.1 Simple type annotation \(STA\)

STA are those that consist of only a single string token identifier. For example, `Int` , `String` , etc. Two STA are considered identical is their identifier is lexically equal.

#### 6.6.2.2 Compound type annotation \(CTA\)

CTA are those that does not only consist of a string token identifier, but also contains inner type annotations. Two CTA are considered identical if their identifier is lexcially equal to each other and their inner types are identical.

The following code snippets demonstrate identical CTA:

```c
List.of(Int)
List.of(Int)
```

```c
Map.key(String) value(Int)
Map.key(String) value(Int)
```

The following code snippets demonstrate non-identical CTA: 

```c
// Different identifer
List.of(Int)
Set.of(Int)
```

```c
// Different inner type
List.of(String)
List.of(Int)
```

#### 6.6.2.3 Bounded type variable \(BTV\)

BTV is primarily used for declaring generic functions. Two BTV are considered identical if their corresponding constraint are identical.

In the following snippet, `A` and `B` are identical BTV despite their name difference, because their constraint is identical.

```text
{A Type}
{B Type}
```

However, in the following snippet, `A` and `A` are considered non-identical BTV because their constraint are different, albeit having identical identifier.

```c
{A Type.where(Comparable)}
{B Type.where(Comparable)}
```

### 6.6.3 Definition of import conflicts

Import conflicts happen when an importer X, where its importees contain identical declarations when compared to each other.

### 6.6.4 Conflicts between importees

Suppose `C` imported `A` and `B` , but both `A` and `B` contains [identical public declarations](section-6-modules.md#6-6-1-identical-declarations), say XA\(declared in A\) and XB\(declared in B\), the compiler should never give any kind of warning or errors, unless an identifier `XC` which is identical to `XA` and `XB` is used in `C` .

 For example, suppose there are two modules as follows:

{% code-tabs %}
{% code-tabs-item title="A.keli" %}
```text
(this Int).plus(that Int) = undefined
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="B.keli" %}
```text
(this Int).plus(that Int) = undefined
```
{% endcode-tabs-item %}
{% endcode-tabs %}



And a module `C` with imported both `A` and `B` as follows:

{% code-tabs %}
{% code-tabs-item title="C.keli" %}
```c
= module.import("A.keli")
= module.import("B.keli")
```
{% endcode-tabs-item %}
{% endcode-tabs %}

When running `C` as entry point, no compiler error or warning will be shown, although `A` and `B` contains identical declarations, which is the `plus` function.

However, if a module `D` imported both `A` and `B` and uses the `plus` function as follows:

{% code-tabs %}
{% code-tabs-item title="D.keli" %}
```c
= module.import("A.keli")
= module.import("B.keli")
= 1.plus(2)
```
{% endcode-tabs-item %}
{% endcode-tabs %}

When running `D` as entry point, the compiler will throw an error saying:

```c
The function `(Int).plus(Int)` is ambiguous, 
as it is declared in both "A.keli" and "B.Keli".
```

## 6.7 Conflict resolution

Import conflicts are sometimes unavoidable, albeit Keli supports multiple dispatch. To resolute conflicts, we can use the `.using` magic function.

Suppose we have the following files:

{% code-tabs %}
{% code-tabs-item title="MathV1.keli" %}
```text
(this Int).square | Int = undefined

```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="MathV2.keli" %}
```text
(this Int).square | Int = undefined

```
{% endcode-tabs-item %}
{% endcode-tabs %}

When we imports those files into another file, say `Main.keli` , no compile error would be introduced.

{% code-tabs %}
{% code-tabs-item title="Main.keli" %}
```c
=module.import("MathV1.keli")
=module.import("MathV2.keli")
// No error
```
{% endcode-tabs-item %}
{% endcode-tabs %}

However, when we attempts to use the `square` function, which is defined in both `MathV1` and `MathV2` with the same parameter types, we will get a compile error:

{% code-tabs %}
{% code-tabs-item title="Main.keli" %}
```c
=module.import("MathV1.keli")
=module.import("MathV2.keli")
=123.square 
// Conflict error: 
// There are 2 definitions of:
//     (_ Int).square
// 
// which are found in:
//    MathV1
//    MathV2
```
{% endcode-tabs-item %}
{% endcode-tabs %}

To resolve the error above, we can use the `.using` function as follows:

{% code-tabs %}
{% code-tabs-item title="Main.keli" %}
```c
=module.import(MathV1)
=module.import(MathV2)
=123.square.using(MathV1)
=456.square.using(MathV2)
=123.square.using(MathV1).square.using(MathV2)
// No error
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% hint style="warning" %}
Warning

In the situation of naming conflicts, one shouldn't use conflict resolution straight away, but should properly investigate if there are code duplication, because it is very rare for two functions to bear the same name and the same parameter type but serves a different purpose. 

Thus, despite using conflict resolution, one could choose to:

1. Remove the duplicated function
2. Rename the function that causes conflicts
{% endhint %}

## 6.9 Restrictions

In this section, we will discuss about the various types of restrictions imposed by the Keli module system. 

### 6.9.1 Importing module with same name

Any Keli source file cannot import two or more modules that have the same name, even though they sits in different directory.

For example, suppose we have the following file structure:

```text
DirA
    Math.keli
DirB 
    Math.keli
Main.keli
```

And in `Main.keli` :

{% code-tabs %}
{% code-tabs-item title="Main.keli" %}
```c
= module.import("DirA/Math.keli")
= module.import("DirB/Math.keli")
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Will result in a compile error, because both imported modules have the same name, which is `Math.keli`.

### 6.9.2 Cyclic imports



