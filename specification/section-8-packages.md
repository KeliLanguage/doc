# Section 8: Packages

## 8.0 Introduction

Packages can be viewed as a collection of [modules](section-6-modules.md).

In Keli, every package is essentially one Git repository. Thus, packaging system of Keli has two external dependencies, namely Git and Git repositories hosting website, such as Github, GitLab, and etc.

## 8.1 Summary

The following items will be discussed in this section:

* How each Keli package should be structured
* How to create a new package
* Structure of the package manifest file
* How to add new dependency
* How to install dependencies
* How to publish a package

## 8.2 Package structure

Each Keli package will have the following structure:

* a `_src` folder, containing all the source file of this package, and a `purse.json` file
* a `_test` folder, containing all the test scripts for testing this package
* `purse.json` file contains a list of dependencies of this package \(refer [Section 8.4](https://github.com/KeliLanguage/doc/tree/8ad3ec5699233d6b2a09273d911b391812abb5ec/specification/section-8-kind-annotations.md#8-4-adding-dependency)\)
* a `.gitignore` file that will ignore every folders\(which are effectively the external dependencies of the current package\), except the `_src` folder.
* a README file, to describe this package
* a LICENSE file

For example, suppose we want to create a package named `Graph`.

{% code-tabs %}
{% code-tabs-item title="Folder Structure 1" %}
```text
Graph/
    .gitignore
    LICENSE
    README.md

    _src/
        toposort.keli
        graph.keli
        purse.json

    _test/
        test1.keli
        test2.keli

    MathOrg.Math.0.0.1/
        purse.json
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
        purse.json
    _test/
    .gitignore
    README.md
    LICENSE
```

The contents of `.gitignore` are as follows:

{% code-tabs %}
{% code-tabs-item title=".gitignore" %}
```bash
# ignore all folders
/*

# except
!.gitignore
!README.md
!LICENSE
!_src/
!_test/
```
{% endcode-tabs-item %}
{% endcode-tabs %}

The format of `purse.json` is explained on the next section.

## 8.4  Manifest file

The manifest file for each Keli package is named `purse.json` . The contents of the file should strictly adhere to the following format:

```javascript
{
    "os"       : "<OS>",
    "arch"     : "<ARCH>",
    "compiler" : "<VERSION>",
    "git"      : "<VERSION>",
    "node"     : "<VERSION>",
    "dependencies": [{
        "url" : "<GRURL>",
        "tag" : "<VERSION>"
    }]
}
```

`<OS>` stands for the name of the operating system, for example Windows, Darwin \(MacOS\), Linux etc.

`<ARCH>` stands for the system architecture, for example `i386` .

Each `<VERSION>` is a semantic versioning string, e.g. `0.0.1` .

Meanwhile, each `<GRURL>` is a valid Git repository URL. Every GRURL must have all of the following characteristics:

1. The name of owner of the Git repository must be present \(either a username, or an organization name\). 
2. The name of the repository must be present.  
3. Must end with `.git` .

Examples of valid GRURL are:

```text
https://gitlab.com/gitlab-org/gitlab-ce.git
https://github.com/red/red.git
```

Basically, every Github or GitLab repository URL can be considered a valid GRURL. 

## 8.5 Adding dependency

To add new dependency, we can simply use the following command:

```text
keli add-dependency <GRURL> <TAG>
```

For example:

```text
keli add-dependency https://github.com/KeliLanguage/corelib 0.0.1
```

By invoking this command, two things will be done:

1. `_src/purse.json` will be updated
2. The process of installing the new package will be triggered

## 8.6 Installing defined dependencies

Dependencies can be installed using the following command:

```text
keli install <path_to_purse.json>
```

For example, to install all the dependencies for the `Graph` package \(refer Folder Structure 1\), we would type the following command inside the `Graph` directory:

```text
keli install _src/purse.json
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
            if($name.existsIn(".")) {
                # no need to install this dependency
            } else {
                runCommand("git clone -b '$tag' --single-branch --depth 1 $url $name")
                fs.moveFilesFrom("$name/_src/*") to("$name/")
                fs.deleteFolder("$name/_src")
                fs.deleteFolder("$name/_test")
                fs.deleteFolder("$name/.git")
                install("$name/purse.json")
            }
        })
}
```

## 8.7 Publishing package

To publish a package, we just need to push our package to a Git repository hosting sites such as GitHub or GitLab. Secondly, it must be tagged using the `git tag` command.

