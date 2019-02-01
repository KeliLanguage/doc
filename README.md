# Motivation

A user from Quora asked: 

> [Why isn't functional programming that popular even though it's so beneficial?](https://www.quora.com/Why-isnt-functional-programming-that-popular-even-though-its-so-beneficial)

One of the top answer says: 

> The user experiences of functional programming languages sucks.

No doubt, this is a sad but true story for most functional programming  \(FP\) languages. I personally thought that FP languages like Haskell offers a very unified and pure programming concepts \(_i.e. everything are functions, and functions and compose well, etc_\). However, Haskell syntax is a mess \([take a look at the grammar](https://www.haskell.org/onlinereport/syntax-iso.html)\), there are a lot of edge cases \(e.g. [weird indentation rules](https://github.com/haskell/haskell-mode/wiki/Why-TAB-cycle-indentation-for-Haskell-is-a-hard-problem)\). Of course this problem is not only limited to Haskell, similar problems can also be found in other FP languages like Erlang, F\#, OCaml etc.  Thus, I believe that these are also the primal reason that most people felt FP languages are hard. 

> I wanted more people to adopt FP.

That is what I wished for, therefore I'm motivated to create a language called Keli \(named after my girlfriend\), which hopes to an FP language with good user experience \(UX\). 

> But what is consider good UX?

If you look at the [TIOBE Index 2018](https://www.tiobe.com/tiobe-index/), you can see that all the top 10 programming languages, most of them has one thing in common, they supports Object-Oriented Programming \(OOP\). Although OOP concepts are not as good as FP concepts, but it surely nailed one aspect, which is the user experience. Why did I say so? Look at the following GIF: 

![IntelliSense in action](.gitbook/assets/intellisense.gif)

> Most of them supported IntelliSense \(a.k.a code completion, code hinting, content assist, etc.\)

One might argue that it is because those languages have their dedicated IDE to support them for such feature, while FP languages don't have as much. The latter might be true, but I would like to also argue that it is very difficult if not impossible to implement IntelliSense for FP languages, **because FP syntax prevented so**, **inherently.**  

Due to this observations, I started a journey to search for the programming language with the most beautiful syntax. Guess what? It's [SmallTalk](https://en.wikipedia.org/wiki/Smalltalk)! It's syntax can even be fitted into a single post card:

![Smalltalk syntax in just a post card](.gitbook/assets/image%20%281%29.png)

Now, try to fit the syntax description of any other programming languages into one post card. I bet you couldn't, so am I. 

Thus, let me present you the [Johari window](https://en.wikipedia.org/wiki/Johari_window) of programming languages: 

![Johari window of programming languages \(sorry if I offended any Perl-ians\)](.gitbook/assets/image%20%282%29.png)

Before I continue, let me clarify some terms, 

* Pure programming model shall means pure FP \(such as Haskell\)
* Pure syntax shall means that it has minimal syntax sugar \(and can easily support Intellisense\)

As we can see,  none of the current programming languages \(at least from what I know\) can fit into the top left corner. 

You might have guessed what I'm going to say next. Yes, you're right. 

> Keli will be the first programming language that incorporates pure syntax with pure  programming model. Double pure!

The end.

