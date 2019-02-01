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
= console.log("hello world")
= file.open("hello.txt")
```

