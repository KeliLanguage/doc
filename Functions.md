# Functions

```js
// unary
a:boolean.not -> boolean = 
    a.ifTrue false ifFalse true

// binary and above
a:boolean.and b:boolean -> boolean =
    a.ifFalse false ifTrue b

a:string.replace old:string with new:string -> string =
    a.splitBy old.joinBy new

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
console.log(x.name.toUpperCase)

// update
y = x.name "lee".age 9
console.log(y) // {name: "wong", }
```

## Binary Tree example

```js

// no idea yet
comparable = interface.
    a .< 

class Comparable a where
    (<) :: a -> a -> Bool



a:comparable.btree -> type = 
    (data.kind #leaf)
    .or
    (data.
        kind    #node
        left    (a.btree)
        current a
        right   (a.btree))

{a:comparable} 
(x:a.btree).insert value:a -> a.btree = 
    x.kind
        .if #leaf then
            (node.
                left    leaf 
                current value 
                right   leaf))
        .if #node then
            (value .< (x.value).
                ifYes (tree.left  (tree.left.insert value))
                ifNot (tree.right (tree.right.insert value))))

{a:comparable}
(x:a.btree).toList -> a.list =
    x.kind
        .if #nil  then []
        .if #cons then (x.left.toList.++[x.current].++(x.right.toList))

{a:comparable}
(x:a.list).toBtree -> a.btree =
    x.kind
        .if #nil  then leaf
        .if #cons then
            (node.
                left    leaf
                current x.current
                right   leaf).insert(x.next)
```