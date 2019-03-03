# Objects

## Simple objects

A simple object can be defined as follows:

```haskell
People = object.
    name(String)
    age (Int)
```

{% hint style="info" %}
Hint

The code above actually is just type aliasing, it means `people` shall means `object.name (String) age (Int)`. 
{% endhint %}

And, to create a object we also use a similar syntax:

```haskell
me = People.name("wong") age(21)
```

To access the fields of a object:

```text
myName = me.name
```

To update a particular field:

```text
olderMe = me.age(me.age.+(10))
```

{% hint style="info" %}
Hint

Every object are immutable in Keli, so updating a particular field of a object will generate a new object instead of mutating the old one.
{% endhint %}

## Anonymous objects

You can actually create a object without actually declaring a type alias for it. For example,

```bash
myDog = object.name("Bibi") color("brown")
```

Then, the compiler will automatically deduce that `myDog` have the type of `object.name(String) color(String)`.

## Row-type polymorphism

Suppose we have the following object:

```haskell
Model = object.
    name    (String)
    age     (Int)
    hobby   (String)
    contact (String)
```

And, we have a function that update the name of a model:

```haskell
(this Model).updateName(newName String) | Model =
    this.name ("<updated>:".++(newName))
```

The code above might seems innocent, but when we want to test this function we will face trouble as we need to create a lot of useless data.

```haskell
mockModel = Model.
    name    ("john")
    age     (99)
    hobby   ("nothing")
    concact ("999")

=mockModel.updateName.name.shouldBe("<updated>:john")
```

To improve the code, we can use row-type polymorphism. Now let us redefine the `updateName` function.

```haskell
{T Type.subtypeOf(object.name(String))}
(this T).updateName(newName String) | T = 
    this.name ("<updated>:".+newName)
```

The code above means, given that we have a type `T` , which is a subtype of `(object.name(String))`, the `updateName` function will take a parameter of type `T` and return a result of type `T` .

Now, to test the `updateName` function, we don't have to create a bunch of unneeded data:

```bash
=object.name("john").updateName.name.shouldBe("<updated>:john")
```

## Object Intersection

In some situation, we might have multiple objects that shares the same fields. For example, suppose we have the following objects:

```haskell
Chef = object.
    name    (String)
    age     (Int)
    address (String)
    food    (String)

Programmer = object.
    name    (String)
    age     (Int)
    address (String)
    language(String)
```

One way to prevent duplication is to use composition, by refactoring out the common fields into a separate object:

```haskell
PersonalInfo = object.
    name    (String)
    age     (Int)
    address (String)
    
Chef = object.
    info (PersonalInfo)
    food (String)

Programmer = object.
    info     (PersonalInfo)
    language (String)
```



Another way to refactor is to use _object intersection_,  by using the `join` function, which can be seen below:

```haskell
PersonalInfo = object.
    name    (String)
    age     (Int)
    address (String)

Chef = PersonalInfo.join(object.food(String))
Programmer = PersonalInfo.join(object.language(String))
```

