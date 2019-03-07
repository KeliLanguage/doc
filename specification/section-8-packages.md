# Section 8: Packages

## 8.0 Introduction

Packages can be viewed as a collection of [modules](section-6-modules.md).

In Keli, every package is essentially one Git repository. Thus, packaging system of Keli has two external dependencies, namely Git and Git repositories hosting website, such as Github, GitLab, and etc.

## 8.1 Summary

The following items will be discussed in this section:

* How each Keli package should be structured
* How to create a new package
* How to add new dependency
* How to install dependencies
* How to publish a package

## 8.2 Package structure

Each Keli package will have the following structure:

* a `_src` folder, containing all the source file of this package, and a `deps` file, and all other miscellaneous files like licences and readme. 
* a `_test` folder, containing all the test scripts for testing this package
* `deps` file contains a list of dependencies of this package \(refer [Section 8.4](section-8-kind-annotations.md#8-4-adding-dependency)\)
* a `.gitignore` file that will ignore every folders\(which are effectively the external dependencies of the current package\), except the `_src` folder.

For example, suppose we want to create a package named `Graph`.

{% code-tabs %}
{% code-tabs-item title="Folder Structure 1" %}
```text
Graph/
    .gitignore
    _src/
        deps
        toposort.keli
        graph.keli
        LICENSE
        README.md
    _test/
        test1.keli
        test2.keli
    MathOrg.Math.0.0.1/
        deps
        numbers.keli
        cartesian.keli
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Each package is structured this way so that there are no implicit mechanism to resolve modules when importing an external module. This contrast with almost all popular programming languages that have a built-in mechanism to import modules from external dependencies.

That is to say, to importing a module from external dependency, we just have to use the same mechanism, which is using relative paths.

For example, based on Folder Structure 1, we can import `numbers.keli` into `graph.keli` by:

{% code-tabs %}
{% code-tabs-item title="graph.keli" %}
```c
= module.import("../MathOrg.Math.0.0.1/numbers.keli")
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Also, by structuring each package this way, we can prevent duplicated modules being fetched, in the case of diamond dependency. For instance, suppose a new package A depends on B and C, while both B and C, depends on D, the package D will not be downloaded twice, as long as B and C depends on the same version of D. In other words, we can achieve a flat dependencies hierarchies, instead of a nested one.

## 8.3 Creating new package

A new Keli package can be created by invoking the compiler CLI command as follows:

```text
keli new-package PackageName
```

Note that the package name should follows the `PascalCase` or `camelCase` convention, and MUST NOT include any symbols except for underscore.

For example, suppose the following command is invoked under the user home directory `~` ,

```text
keli new-package MyPackage
```

Then, a folder named `MyPackage` will appear under the `~` directory, as such:

```text
MyPackage/
    _src/
        deps
    _test/
    .gitignore
```

The contents of each file are:

{% code-tabs %}
{% code-tabs-item title="deps" %}
```text
https://github.com/KeliLanguage/corelib.git[0.0.1]
```
{% endcode-tabs-item %}

{% code-tabs-item title=".gitignore" %}
```bash
# ignore all folders
/*

# except
!.gitignore
!_src/
!_test/
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## 8.4 Adding dependency

Based on Folder Structure 1, we can add new dependency to the `Graph` package by editing the file `_src/deps` .

The contents of `deps` are are effectively a list of Git repository URL, with each suffixed by a tag string enclosed in square brackets. For convenience purpose, such URL will be called as KPURL \(Keli Package URL\) in the following writings.

KPURL can be any URL that points to a Git repository, however, they must fulfill all the following criteria to be considered a valid KPURL:

1. The name of owner of the Git repository must be present \(either a username, or an organization name\). 
2. The name of the repository must be present.  
3. Must end with `.git` .
4. The tag string must be present.

By default, every Github or GitLab repository URL already fulfills criterion 1, 2 and 3. To fulfill criterion 4, additional typing needs to be done.

The following mock URLs are example valid KPURL:

```text
https://gitlab.com/gitlab-org/gitlab-ce.git[11.9.0]
https://github.com/red/red.git[0.6.4]
```

In a nutshell, adding new dependency means to append the `_src/deps` file with a valid KPURL.

## 8.5 Installing defined dependencies

Dependencies can be installed using the following command:

```text
keli install <path_to_deps>
```

For example, to install all the dependencies for the `Graph` package \(refer Folder Structure 1\), we would type the following command inside the `Graph` directory:

```text
keli install _src/deps
```

The following pseudocode shall describe how the dependency installation algorithm works:

```bash
install($depPath) {
    $kpurls = fs.readAllLines($depPath)
    $kpurls
        .forEach(validate)
        .forEach($url -> {
            {$authorName, $repoName, $tag} = extractNames($url)
            $name = "$authorName.$repoName.$tag"
            runCommand("git clone $url $name")
            fs.moveFilesFrom("$name/_src/*") to("$name/")
            fs.deleteFolder("$name/_src")
            install("$name/deps")
        })
}
```

## 8.6 Publishing package

To publish a package, we just need to push our package to a Git repository hosting sites such as GitHub or GitLab. Secondly, it must be tagged using the `git tag` command.

