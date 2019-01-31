---
description: This page shall describe how record types are defined and used in Keli.
---

# Records

## Simple records

A simple record can be defined as follows:

```haskell
People = record.
    name(String)
    age (Int)
```

{% hint style="info" %}
Hint

The code above actually is just type aliasing, it means `people` shall means `record.name (String) age (Int)`. 
{% endhint %}

And, to create a record we also use a similar syntax:

```haskell
me = People.name("wong") age(21)
```

To access the fields of a record:

```text
myName = me.name
```

To update a particular field:

```text
olderMe = me.age(me.age.+(10))
```

{% hint style="info" %}
Hint

Every record are immutable in Keli, so updating a particular field of a record will generate a new record instead of mutating the old one.
{% endhint %}

## Anonymous records

You can actually create a record without actually declaring a type alias for it. For example,

```bash
myDog = record.name("Bibi") color("brown")
```

Then, the compiler will automatically deduce that `myDog` have the type of `record.name(String) color(String)`.

## Row-type polymorphism

Suppose we have the following record:

```haskell
Model = record.
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
{T Type.subtypeOf(record.name(String))}
(this T).updateName(newName String) | T = 
    this.name ("<updated>:".+newName)
```

The code above means, given that we have a type `T` , which is a subtype of `(record.name(String))`, the `updateName` function will take a parameter of type `T` and return a result of type `T` .

Now, to test the `updateName` function, we don't have to create a bunch of unneeded data:

```bash
=record.name("john").updateName.name.shouldBe("<updated>:john")
```

## Record Intersection

In some situation, we might have multiple records that shares the same fields. For example, suppose we have the following records:

```haskell
Chef = record.
    name    (String)
    age     (Int)
    address (String)
    food    (String)

Programmer = record.
    name    (String)
    age     (Int)
    address (String)
    language(String)
```

One way to prevent duplication is to use composition, by refactoring out the common fields into a separate record:

```haskell
PersonalInfo = record.
    name    (String)
    age     (Int)
    address (String)
    
Chef = record.
    info (PersonalInfo)
    food (String)

Programmer = record.
    info     (PersonalInfo)
    language (String)
```



Another way to refactor is to use _record intersection_,  by using the `join` function, which can be seen below:

```haskell
PersonalInfo = record.
    name    (String)
    age     (Int)
    address (String)

Chef = PersonalInfo.join(record.food(String))
Programmer = PersonalInfo.join(record.language(String))
```

