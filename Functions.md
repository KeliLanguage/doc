# Functions

```js
// unary
a:boolean.not -> boolean = 
    a.if_true false if_false true

// binary and above
a:boolean.and b:boolean -> boolean =
    a.if_true b if_false false

a:string.replace old:string with new:string -> string =
    a.split_by old.join_by new

// precedence
// just from left to right, nothing special
true.ifTrue(true)ifFalse(true.and(false.not))
```

# Record type

```js
// definition
people = data.
    kind   #people
    name   string
    age    integer
    friend (people.maybe)

// construction (longer version)
x = people.name "Hello" age 9 friend nil

// construction (shorter version)
x = people.new("hello", 9, nil)

// access
console.log(x.name.to_upper)

// update
y = x.name "lee".age 9
console.log(y) // {name: "wong", }
```

## Binary Tree example

```php

// defining an interface
comparable = interface.new

// defining what functions need to be defined in an interface
<a:comparable>
x:a .> y:a -> boolean = undefined

<a:comparable>
x:a .== y:a -> boolean = undefined

// defining default implementations for some functions
<a:comparable>
x:a .!= y:a -> boolean = x.== y.not

color = data.
    kind #color
    r    integer
    g    integer
    b    integer

// stating that a data should implement an interface
color.implements comparable 

// implementing
x:color .== y:color -> boolean = x.r .== (y.r)



a:comparable.btree -> type = 
    (data.
        kind    #leaf)
    .or
    (data.
        kind    #node
        left    (a.btree)
        current a
        right   (a.btree))

// The following things will be generated automaticall
//  0) Constructors
//      0.1) Long constructor
//      0.2) Short constructor
//  1) Getters
//  2) Setters
//  3) Switchers (e.g. if_xxx if_yyy)

<a:comparable>
(x:a.btree).insert value:a -> a.btree = 
    x.
        if_leaf (node.new(leaf, value, leaf))
        if_node 
            (value .< (x.value).
                if_true  (x.left  (x.left.insert value))
                if_false (x.right (x.right.insert value)))

<a:comparable>
(x:a.btree).to_list -> a.list =
    x.
        if_nil  (a.list.new)
        if_cons (x.left.to_list.append(x.current).concat(x.right.toList))

<a:comparable>
(x:a.list).to_btree -> a.btree =
    x.
        if_nil  leaf
        if_cons (node.new(leaf, x, leaf).insert(x.next))
```

## Lambdas

```php
<a:type, b:type>
xs:(a.list) .map f:(a.to b) -> b.list =
    xs.
        if_nil  []
        if_cons [f.apply(xs.current)].concat(xs.next.map(f)))

y = [1,2,3].map($ + 2)
z = [1,2,3].map((x) -> x + 2)

// single parameter lambda
f = $ + 2 // or
f = ((x) -> x + 2)

// double parameter lambda
f = $ + $$ // or
f = ((x, y) -> x + y)
```

## Monads

```hs
class Monad m where
  (>>=)  :: m a -> (  a -> m b) -> m b
  (>>)   :: m a ->  m b         -> m b
  return ::   a                 -> m a
  fail   :: String -> m a

monad = interface.new

{m:monad}
x:(a.m) .bind f:(a)

```