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

> Most of them supported IntelliSense \(a.k.a code completion, code hinting, content assist, etc.\)

One might argue that it is because those languages have their dedicated IDE to support them for such feature, while FP languages don't have as much. The latter might be true, but I would like to also argue that it is very difficult if not impossible to implement IntelliSense for FP languages, **because FP syntax prevented so**, **inherently.**  

Due to this observations, I started a journey to search for the programming language which is _IDE-friendly and syntactically easy to understand_. 

At first, I looked at C++, Java and C\#, I found out that their syntax definitely made the code more comprehensible when compared to Haskell-like languages due to the postfixity and infixity of methods:

```java
// Java
123.toString()
"Hello world".splitBy(" ")
```

```haskell
-- Haskell
toString 123
splitBy " " "Hello world"
```

So, why did I say Java-like languages is more comprehensible when compared to Haskell-like languages? 

Because in Haskell-like languages \(which favors prefix functions\),  **function arguments position is ambiguous**.

For example, the user will never know whether which of the following produces their intended result without looking at documentation:

```haskell
splitBy "," "Hello,world" -- Should it be this one?
splitBy "Hello,world" "," -- or this?
```

Because of this nature, Haskell-like languages are no syntactically higher level than assembly languages, which looks something like this:

```text
MOV EDX, 5
LDR r8, [r10]
```



Thus, let me present you the [Johari window](https://en.wikipedia.org/wiki/Johari_window) of programming languages: 

![Johari window of programming languages \(sorry if I offended any Perl-ians\)](.gitbook/assets/image%20%282%29.png)

Before I continue, let me clarify some terms, 

* Pure programming model shall means pure FP \(such as Haskell\)
* Pure syntax shall means that it has minimal syntax sugar \(and can easily support Intellisense\)

As we can see,  none of the current programming languages \(at least from what I know\) can fit into the top left corner. 

You might have guessed what I'm going to say next. Yes, you're right. 

> Keli will be the first programming language that incorporates pure syntax with pure  programming model. Double pure!

The end.

