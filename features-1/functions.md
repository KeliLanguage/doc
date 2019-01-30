---
description: This page shall describe how functions is created and invoked in Keli.
---

# Functions

There are two kinds of functions in Keli, 

* Monofunc \(function that takes one and only one parameter\)
* Polyfunction \(function that takes more than one parameters\)

## Foreword

Functions in Keli are declared in a quite unusual fashion:

> Function parameter comes before function identifier.

The reason for this is such that IntelliSense can be implemented easily for this language. 

Not only that, it also makes Keli sounds more like verbal English. Most verbal English sentences starts with an subject, followed by an action. For example,

```text
john      , take me to the hospital
<subject>   <action>
```

I guess this is one of the primal reason why OOP's syntax are easier to grasp.

{% hint style="info" %}
Tips

You can think of dot `.` in Keli as the Unix pipe operator `|`.
{% endhint %}

## Monofunc 

All monofunc in Keli can be declared using the following format:

```text
<parameter> . <function_identifier> | <return_type> = <expr>;
```

Suppose you want to create a function that calculate the factorial of a number:

```java
(x int).factorial | int = 
    x.<= 1.
        true? 1
        false? (x.*((x.-1).factorial)); 
```

Let us dissect it:

| Literal | Term |
| :--- | :--- |
| `(x int)` | Parameter |
| `factorial` | Function identifier |
| `| int` | Return type |

What does it mean? It means this function is called `factorial`, and its parameter is `x` which have the type of `int`. 

Now that we know how to create a monofunc, but how do we use it \(or invoke it\)? 

It is fairly simple, as the following:

```java
x = 2.factorial
```

You can even chain it as long as you like:

```java
x = 2.factorial.factorial.factorial
```

{% hint style="info" %}
About precedence

Every expression in Keli is read from left to right, there are no operator precedence and whatsoever except for parenthesis/bracket precedence,  so the previous function is equivalent to the following :

```java
x = ((2.factorial).factorial).factorial
```
{% endhint %}

## Polyfunc

All polyfuncs in Keli adhere to the following format:

```text
<param> . [<func_id> <param>] | <return_type> = <expr> ;
```

Suppose we want to create a power function:

```java
(base int).power(exp int) | int = undefined;
```

In this case, `base` and `exp` are the parameters of this function, which have the name `power`, and it should return an `integer`. 

Here's how we use the `power` function:

```text
x = 2.power 3
```

Similarly, you can chain it like monofunc:

```text
x = 2.power 3.power 6
```

In C language, the expression above would be written as:

```text
x = power(power(2, 3), 6)
```

{% hint style="warning" %}
Warning

As mentioned earlier, everything is strictly read from left to right in the absence of parenthesis or brackets, so the expression above is equivalent to:

```text
x = (2.power 3).power 6
```

Therefore, if you wish to calculate `3, power 6` first, you should enclose it with parenthesis, as follows:

```text
x = 2.power (3.power 6)
```
{% endhint %}

### Function with more than 2 parameters?

How about functions with more than 2 parameters? This is also very trivial in Keli. Suppose we want to create a function that replace some string with some other string in a string:

```bash
(x str).replace(old str) with(new str) | str = undefined
```

And here's how you would invoke it:

```java
answer = "Hello world".replace "world" with "keli"
```

As always, you could chain it:

```java
answer = "Hello world"
    .replace "world" with "keli"
    .replace "Hello" with "yo"
```

{% hint style="info" %}
Notes

The indentation is **not necessary**, it's just for beauty purpose. 
{% endhint %}

## Custom operators

In fact, it is very easy to create custom operators in Keli, because operators are treated the same as function identifiers! 

{% hint style="info" %}
Any combination of symbols can be used, as long as they are not the [reserved operators](../syntax.md#reserved-operator).
{% endhint %}



Let's look at some example,

```java
="factorial function declaration"
(x int).! | int = undefined

="factorial function invocation"
answer = 5.!

="plus function declaration"
(x int).+(y int) | int = undefined

="plus function invocation"
answer2 = 999.+ 777

="crazy function declaration"
(x int).# (y int) @ (z int) | int = undefined

="crazy function invocation"
123.# 3 @ 4
```



