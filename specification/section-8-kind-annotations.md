# Section 8: Packages

## 8.0 Introduction

Packages can be viewed as a collection of [modules](section-6-modules.md). 

The packaging system of Keli has two external dependencies, namely Git and Git repositories hosting website, such as Github, GitLab, and etc.

## 8.1 Summary

The following items will be discussed in this section:

* How each Keli package should be structured
* How to create a new package
* How to add new dependency
* How to install dependencies
* How to publish a package

## 8.2 Package structure

Each Keli package will have the following structure:

* a `_files` folder, containing all the source file of a particular package, and a `deps` file
* `deps` file contains a list of dependencies, which are effectively a list of Git source URL \(usually GitHub URL\)
* a `.gitignore` file that will ignore every folders\(which are effectively the external dependencies of the current package\), except the `_files` folder.

For example, suppose we want to create a package named `Graph`.

```text
Graph/
    _files/
        deps
        toposort.keli
        graph.keli
    .gitignore
    Math_0_0_1/
        deps
        numbers.keli
        cartesian.keli
```

Each package is structured this way so that there are no implicit mechanism to resolve modules when importing an external module. This contrast with almost all popular programming languages that have a built-in mechanism to import modules from external dependencies.

That is to say, to importing a module from external dependency, we just have to use the same mechanism, which is using relative paths.

For example, we can import `numbers.keli` into `graph.keli` by:

{% code-tabs %}
{% code-tabs-item title="graph.keli" %}
```c

= module.import("../Math_0_0_1/numbers.keli")
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Also, by structuring each package this way, we can prevent duplicated modules being fetched, in the case of diamond dependency. For instance, suppose a new package A depends on B and C, while both B and C, depends on D, the package D will not be downloaded twice, as long as B and C depends on the same version of D. In other words, we can achieve a flat dependencies hierarchies, instead of a nested one. 

## 8.3 Creating new package

A new Keli package can be created by invoking the compiler CLI command as follows:

```text
keli new-package PackageName
```

Note that the package name should follows the `PascalCase` convention.

For example, suppose the following command is invoked under the user home directory `~` , 

```text
keli new-package MyPackage

```

