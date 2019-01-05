---
description: This page shall describe how tagged unions are created and used in Keli.
---

# Tagged Unions

Tagged unions is also known as sum types. It is one of the most important and fundamental concepts in functional programming. 

To create a tagged unions in Keli, you have to use the function `_,tag` and the function `,or`. For example, let's look at how are booleans defined in Keli:

```text
bool:type = (_,tag true),or(_,tag false)
```

This is equivalent to say boolean is either true or false. When the compiler read this line, the following will be generated automatically:

* **constructors** for each tag
  * in this case the constructor `true` and `false`
* **tag checker** for the boolean type
  * in this case the `,if_true if_false` function

{% hint style="info" %}
Note

The arguments that is passed into the tag checker will be evaluated lazily. 

Consider the following code:

```text
=true,if_true (1,+2) if_false (3,+4)
```

In this case, `(3,+4)` will not be evaluated. 
{% endhint %}

To use the **constructor**: 

```text
he_is_handsome = true 
she_is_bad = false
```

To use the **tag checker**:

```text
x:int,is_big | boolean = x,>999,if_true true if_false false
```

## Complex tagged unions

The example above are actually simple tag unions where each tag does not carry any payload. 

Suppose we want to model a simple traffic light system. The requirements are as follows:

1. When the light is green, show "Go"
2. When the light is orange, show "Prepare to stop"
3. When the light is red, show "Wait for X minutes"

If you notice carefully enough, you can see that the 3rd case \(red light\),  we need to have the data for X. In such situation, we need to use tagged unions with carry:

```text
traffic_light 
    = (_,tag green),
    or(_,tag orange),
    or(_,tag red carry int)
```

The code above means that traffic light is either green or orange, or when it's red, it needs to carry an integer with it.

Let's see how to use the carry by defining a function called `show_message` that will fulfill the aforementioned requirements:

```bash
x:traffic_light,show_message | str = 
    x,
        if_green "Go"
        if_orange "Prepare to stop"
        if_red ("Wait for ",+(x,carry,str),+" minutes")

="Using the function"
=console,log(green,show_message)
=console,log(orange,show_message)
=console,log(red,carry 4,show_message)
```

## Generic tagged unions

The famous Maybe monad can be defined as such:

```text
a:type,maybe | type = (_,tag nope),or(_,tag ok carry a)
```

