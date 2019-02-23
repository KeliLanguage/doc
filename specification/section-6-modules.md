# Section 6: Modules

As mentioned in [Section 1](chapter-1-introduction.md), a Keli program is actually a set of modules. In fact, each module actually corresponds to one Keli file. 

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

## 6.4 Imports

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



## 6.5 Example 

Suppose we have the following folder structure:

```text
- myapp
    - Main.keli
    - Src
        - Math.keli
        - Misc
            - Shape.keli
```

To import `Math` and `Shape` into `Main` :

{% code-tabs %}
{% code-tabs-item title="Main.keli" %}
```c
=module.import("Src/Math")
=module.import("Src/Misc/Shape")
```
{% endcode-tabs-item %}
{% endcode-tabs %}

To import `Math` into `Shape` :

{% code-tabs %}
{% code-tabs-item title="Shape.keli" %}
```c
=module.import("../Math.keli")
```
{% endcode-tabs-item %}
{% endcode-tabs %}

If we had declared that `$projectRoot` as `./` , then we can rewrite the code above as:

{% code-tabs %}
{% code-tabs-item title="Shape.keli" %}
```c
=module.import("$projectRoot/Src/Math.keli")
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## 6.6 Conflict resolution

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
=module.import(MathV1)
=module.import(MathV2)
// No error
```
{% endcode-tabs-item %}
{% endcode-tabs %}

However, when we attempts to use the `square` function, which is defined in both `MathV1` and `MathV2` with the same parameter types, we will get a compile error:

{% code-tabs %}
{% code-tabs-item title="Main.keli" %}
```c
=module.import(MathV1)
=module.import(MathV2)
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

