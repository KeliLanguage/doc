# Motivation

A user from Quora asked: 

> [Why isn't functional programming that popular even though it's so beneficial?](https://www.quora.com/Why-isnt-functional-programming-that-popular-even-though-its-so-beneficial)

One of the top answer says: 

> The user experiences of functional programming languages sucks.

No doubt, this is a sad but true story for most functional programming  \(FP\) languages. I personally thought that FP languages like Haskell offers a very unified and pure programming concepts \(_i.e. everything are functions, and functions and compose well, etc_\). However, Haskell syntax is a mess \([take a look at the grammar](https://www.haskell.org/onlinereport/syntax-iso.html)\), there are a lot of edge cases \(e.g. [weird indentation rules](https://github.com/haskell/haskell-mode/wiki/Why-TAB-cycle-indentation-for-Haskell-is-a-hard-problem)\). Of course this problem is not only limited to Haskell, similar problems can also be found in other FP languages like Erlang, F\#, OCaml etc.  Thus, I believe that these are also the primal reason that most people felt FP languages are hard. 

> I wanted more people to adopt FP.

That is what I wished for, therefore I'm motivated to create a language called Keli \(named after my girlfriend\), which hopes to an FP language with good user experience \(UX\). 

In the following section, I will describe the UX problems of FP languages and how they are solved in other programming languages.

### 1. Ambiguous functions argument positions

This is a problem where the user cannot properly tell where to place function arguments properly without looking at documentation. This problem is actually prevalent for every programming languages that favors _prefix function invocations ,_ for example C, Bash, Assembly, and most FP languages. 

For example, without referring to documentation or previous experiences, a user can never tell which of the following will yield intended result, even though the user have the domain knowledge \(in this case, basic English\).

```haskell
-- Is this correct?
splitBy ","  "1,2,3,4,5"

-- or this?
splitBy "1,2,3,4,5"  ","
```

This problem is slightly mitigated in object-oriented programming\(OOP\) languages due to their _infix function invocation_ nature. For example, in Java or Scala, the code above can be rewritten in a more comprehensible manner:

```java
// This is obviously not too right
",".splitBy("1,2,3,4,5")

// This should be right, because it reads out more naturally
"1,2,3,4,5".splitBy(",")
```

However, OOP languages also suffers the same problem whenever the number of function/method arguments get more than two, for example:

```java
// Which one is right?
"Hello world".replace("Hello", "Bye")
"Hello world".replace("Bye", "Hello")
```

Fortunately, this problem is actually solved by a languages called Smalltalk, which is designed by Alan Kay and intended to be used by children of all ages. Smalltalk solves this by using its message syntax, for example:

```java
'Hello world' replace: 'Hello' with: 'Bye'
```

And the good thing of such syntax is that it scales, no matter how many arguments your function is taking, for example:

```java
'Hello world' replaceFromIndex: 0 toIndex: 4 with: 'Bye'
```

Therefore, Keli shall adopts the message syntax from Smalltalk.

### 2. Not Intellisense friendly

The other problem of FP languages is that they are not Intellisense friendly, in another word, they don't integrate well with integrated development environment \(IDE\). 

This is also due to their prefix function invocation nature, which does not allow Intellisense to provide good code completions suggestion. The best they can do is to provide some low-level Intellisense such as:

* Naively suggesting variable or function names 
* Suggesting function when typing out a module name

This kind of Intellisense are dwarfed when compared to those of OOP languages, which provide higher-level Intellisense, which is:

* Suggesting related method based on type of the object

![Higher-level IntelliSense in action](.gitbook/assets/intellisense.gif)

Most of them supported IntelliSense \(a.k.a code completion, code hinting, content assist, etc.\) 

### Conclusion

In a nutshell, Keli strives to fill the hole in the table below, which is to be a functional programming language that is IDE-friendly.

|  | Functional Programming | Non-functional programming |
| :--- | :--- | :--- |
| IDE-friendly | ? | C\#, Smalltalk, Java |
| Not IDE-friendly | Haskell, F\#, OCaml | Perl, C, Bash |





