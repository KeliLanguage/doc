# Section 1: Introduction

## 1.0 Introduction

Keli is a general purpose programming language where its semantics are mostly based on the pure functional programming model \(as of [Haskell's](https://www.haskell.org/)\) which includes user-defined algebraic datatypes, case matching and higher-order functions, while its syntactic structure is strongly influenced by [Smalltalk's message sending syntax](http://pharo.gforge.inria.fr/PBE1/PBE1ch5.html), such as the unary, binary and keywords messages. 

Moreover, it is almost [homoiconic](https://en.wikipedia.org/wiki/Homoiconicity), in the sense that all non-literal expression are functions invocation, including record definition, control structures, type annotation etc.

Keli is created to overcome a problem that is almost undeniable in every functional programming languages: the lack of proper IDE incorporation \(perhaps except F\#\). In short, Keli can be viewed as an attempt to assimilate the Smalltalk's syntactic model into a functional programming environment. 

The template of this report is based on the [Haskell2010 Language Specification Report](https://www.haskell.org/onlinereport/haskell2010/), which shall define the syntax for Keli programs and semantics explanation for each features of Keli.

## 1.1 Program Structure

1. At the topmost level, a Keli program is a set of modules, as described in Section 6. Modules is used for organizing components in a large program to improve software re-usability.
2. Each module contains a list of declarations, as described in Section 5.
3. Each declarations are made up of expressions, as described in Section 3 where each expression denotes a value and bear a static type \(which can be user-annotated or inferred by the compiler\). Also, there are magic expressions which will trigger the internal mechanism of the Keli compiler \(also discussed in Section 4\).
4. At the lowest level is the Keli's lexical structure, as defined in Section 2. Lexical structure shall define how a Keli program can be represented textually.



