# Singleton

## Purpose

Sometimes, when we are creating libraries for other users, we might want to group similar functions together to improve user experience.

## Example

Suppose we are creating a library that allow the user to access the file system. And we wish that our code looks like this:

```haskell
=fs.open("Hello world.txt")
```

First, we need to create a [tagged union](../features-1/tagged-unions.md) with only a single tag:

```haskell
FileSystem = tag.fs
```

Then, create a constant alias:

```haskell
fs = FileSystem.fs
```

After that, we can proceed to define the functions we intend to expose:

```haskell
(this FileSystem).open(filename String) = undefined
(this FileSystem).rename(oldname String) as(newname String) = undefined
(this FileSystem).delete(filename String) = undefined
```

And the client can use them as follows:

```haskell
=fs.open("Hello world.txt")
=fs.rename("Jojo.md") as("Koko.md")
=fs.delete("/")
```

