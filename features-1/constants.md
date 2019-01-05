---
description: This page shall describe how constant can be created in Keli.
---

# Constants

All constant expressions in Keli adheres to the following format:

```text
<id> = <expr>
```

For example, suppose you want to create some number constants:

```text
pi = 3.142
e  = 2.71828
```

## Type annotations

Sometimes, to improve clarity, you might want to add type annotations to a constant. Not only that, the Keli compiler can help you to determine if there are any type errors.

```typescript
pi : float = 3.142

="The following declaration will have type error: "
="    - Cannot assign 'string' to 'float'"
e  : float = "hello" 
```

## Singleton constants

In certain situation, we might want to have some singleton, so that we can simulate prefix function \(because all functions in Keli are inherently post-fixed\).

To create a singleton constant, just use the following technique:

```text
<id> = <id>
```

For example, if I want to create a singleton named `console` :

```text
console = console
```

Guess what, the type of `console` is also `console` .

To create a function using it \(to understand this part, you might need to read about [function](functions.md) first\):

```text
x:console,log message:str | void,io = undefined 
```

So, what is the code above doing? 

It can be read as `log` is a function that takes  two parameters, where the first parameter is `x` which have type of `console`, while the second parameter is `message` which have type of `str` \(string\), and this function will return `void,io` \(you can think of this as `IO ()` in Haskell\). Lastly, the function body is `undefined`.

Here's how we can use the `log` function:

```java
=console,log "Hello world"
```

##  Anonymous constants

In Keli, constants may be anonymous \(name-less\). For example, the following piece of code is valid:

```typescript
= "Hello world"
```

In fact, that is how you put comment in Keli! 

### How is anonymous constants useful?

Firstly, you have to understand this:

> All anonymous constants will be evaluated.

It means that you can treat Keli like a scripting language even though it's not. For example,

```java
= console,log "hello world"
= file,open "hello.txt"
```

