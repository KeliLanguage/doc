---
description: This page shall describe how record types are defined and used in Keli.
---

# Records

## Simple records

A simple record can be defined as follows:

```text
people = record.
    name str 
    age int
```

{% hint style="info" %}
Hint

The code above actually is just type aliasing, it means `people` shall means `record.name str age int`. 
{% endhint %}

And, to create a record we also use a similar syntax:

```bash
me:people = record.name "wong" age 21
```

To access the fields of a record:

```text
myName = me.name
```

To update a particular field:

```text
olderMe = me.age (me.age.+10)
```

{% hint style="info" %}
Hint

Every record are immutable in Keli, so updating a particular field of a record will generate a new record instead of mutating the old one.
{% endhint %}

## Anonymous records

You can actually create a record without actually declaring a type alias for it. For example,

```bash
myDog = record.name "Bibi" color "brown"
```

Then, the compiler will automatically deduce that `myDog` have the type of `record.name str color str`.

## Row-type polymorphism

Suppose we have the following record:

```text
model = record.
    name    str
    age     int
    hobby   str
    contact str
```

And, we have a function that update the name of a model:

```bash
this:model.updateName newName:str | model =
    this.name ("<updated>:".+newName)
```

The code above might seems innocent, but when we want to test this function we will face trouble as we need to create a lot of useless data.

```bash
mockModel:model = record, 
    name    "john"
    age     99
    hobby   "nothing"
    concact "999"

=mockModel.updateName.name.shouldBe "<updated>:john"
```

To improve the code, we can use row-type polymorphism. Now let us redefine the `updateName` function.

```bash
{a:(record.name str)}
this:a.updateName newName:str | a = 
    this.name ("<updated>:".+newName)
```

The code above means, given that we have a type `a` , which is a subtype of `(record.name str)`, the `updateName` function will take a parameter of type `a` and return a result of type `a` .

Now, to test the `updateName` function, we don't have to create a bunch of unneeded data:

```bash
=record.name "john".updateName.name.shouldBe "<updated>:john"
```

## Record Intersection

In some situation, we might have multiple records that shares the same fields. For example, suppose we have the following records:

```text
chef = record.
    name    str
    age     int
    address str
    food    str

programmer = record.
    name     str
    age      int
    address  str
    language str
```

One way to prevent duplication is to use composition, by refactoring out the common fields into a separate record:

```text
personalInfo = record.
    name    str
    age     int
    address str
    
chef = record.
    info personalInfo
    food str

programmer = record.
    info     personalInfo
    language str
```



Another way to refactor is to use _record intersection_,  by using the `join` function, which can be seen below:

```text
personalInfo = record.
    name    str
    age     int
    address str

chef = personalInfo.join(record.food str)
programmer = personalInfo.join(record.language str)
```

