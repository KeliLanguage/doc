---
description: This page shall describe how tagged unions are created and used in Keli.
---

# Tagged Unions

Tagged unions is also known as sum types. It is one of the most important and fundamental concepts in functional programming. 

To create a tagged unions in Keli, you have to use the function \_`.tag` and the function `.or`. For example, let's look at how are booleans defined in Keli:

```text
bool:type = (_.tag true).or(_.tag false)
```

This is equivalent to say boolean is either true or false. When the compiler read this line, the following will be generated automatically:

* **constructors** for each tag
  * in this case the constructor `true` and `false`
* **tag checker** for the boolean type
  * in this case the `.true? false?` function

{% hint style="info" %}
Note

The arguments that is passed into the tag checker will be evaluated lazily. 

Consider the following code:

```text
=true.true? (1.+2) false? (3.+4)
```

In this case, `(3.+4)` will not be evaluated. 
{% endhint %}

To use the **constructor**: 

```text
heIsHandsome = true 
sheIsBad = false
```

To use the **tag checker**:

```text
x:int.isBig | boolean = x.>999.true? true false? false
```

{% hint style="info" %}
## Optional case checker

Sometimes we only care about certain tags, in such situation we can use the `else?` tag.  
  
For example, suppose we have the tagged union:

```text
animal 
    =  (_.tag bird)
    .or(_.tag mammal)
    .or(_.tag fish)
    .or(_.tag insect)
```

Let say we want to have a function that check whether an animal is a bird, we can implement it naively as follows:

```text
this:animal.isBird | bool =
    this.
        bird?   true
        mammal? false
        fish?   false
        insect? false
```

The function above works just fine, however there is just too much typing, and if we add new animal tag in the future, we need to modify this function as well. 

To improve the code, we can use the `else?` tag:

```text
this:animal.isBird | bool = 
    this.
        bird? true 
        else? false
```
{% endhint %}





## Complex tagged unions

The example above are actually simple tag unions where each tag does not carry any payload. 

Suppose we want to model a simple traffic light system. The requirements are as follows:

1. When the light is green, show "Go"
2. When the light is orange, show "Prepare to stop"
3. When the light is red, show "Wait for X minutes"

If you notice carefully enough, you can see that the 3rd case \(red light\),  we need to have the data for X. In such situation, we need to use tagged unions with carry:

```text
trafficLight 
    =  (_.tag green)
    .or(_.tag orange)
    .or(_.tag red carry int)
```

The code above means that traffic light is either green or orange, or when it's red, it needs to carry an integer with it.

{% hint style="info" %}
Hint

In general, to define a tag with carry, we use the following format:

```text
_.tag <tagId> carry <type>
```

`<tagId>` means any valid [identifiers](../syntax.md#identifiers), while `<type>` means any valid [types](types.md).
{% endhint %}

Let's see how to use the carry by defining a function called `showMessage` that will fulfill the aforementioned requirements:

```bash
x:trafficLight.showMessage | str = 
    x.
        green?  "Go"
        orange? "Prepare to stop"
        red?   ("Wait for ".+(x.carry.str).+" minutes")

="Using the function"
=console.log(green.showMessage)
=console.log(orange.showMessage)
=console.log(red.carry 4.showMessage)
```

## Generic tagged unions

The famous Maybe monad can be defined as such:

```text
a:type.maybe | type = (_.tag nothing).or(_.tag just carry a)
```
