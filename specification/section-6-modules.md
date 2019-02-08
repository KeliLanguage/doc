# Section 6: Modules

As mentioned in [Section 1](chapter-1-introduction.md), a Keli program is actually a set of modules. In fact, each module actually corresponds to one Keli file. For example, if the file is named `App.keli` , then the module name is `App` . 

## 6.1 The Main module

Every Keli program must have a `Main` module, which can be created by naming a file `Main.keli`. This module shall serves as the root of each project. 

## 6.2 Module naming convention

The Keli compiler shall warn the user if the module naming does not follows the `PascalCase` convention.

## 6.3 Encapsulation

By default, all declarations in a module are accessible by any other modules. To achieve encapsulation, Keli adopts a rather tolerant encapsulation mechanism, where the compiler would not trigger any compile error, but warnings instead. 

To make a declaration invisible to other module, prefix the declaration with an underscore symbol. For example,

```c
_pi = 3.142 // invisible to other module
(this Number)._square = undefined // this is invisible to other module
```

However, this does not means that the `_square` function cannot be used by other modules, it will only trigger compiler warning if the compiler found that other modules are using them.

## 6.4 Imports

To import a module, we use the magic function `module.import` , for example,

```text
=module.import(Math)
```

## 6.5 Folder structure

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
```text
=module.import(Src.Math)
=module.import(Src.Misc.Shape)
```
{% endcode-tabs-item %}
{% endcode-tabs %}

The import semantics of Keli follows the project root path instead of relative path, so to import `Math.keli` into `Shape.keli` , we also use `Src.Math` , as follows:

{% code-tabs %}
{% code-tabs-item title="Shape.keli" %}
```text
=module.import(Src.Math)
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

