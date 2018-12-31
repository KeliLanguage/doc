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

```js

// no idea yet
comparable = interface.
    a .< 

class Comparable a where
    (<) :: a -> a -> Bool

instance Comparable Color where
    a < b = (red a) < (red b)




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

{a:comparable} 
(x:a.btree).insert value:a -> a.btree = 
    x.
        if_leaf (node.new(leaf, value, leaf))
        if_node 
            (value .< (x.value).
                if_true  (x.left  (x.left.insert value))
                if_false (x.right (x.right.insert value)))

{a:comparable}
(x:a.btree).to_list -> a.list =
    x.
        if_nil  []
        if_cons (x.left.to_list.append(x.current).concat(x.right.toList))

{a:comparable}
(x:a.list).to_btree -> a.btree =
    x.
        if_nil  leaf
        if_cons (node.new(leaf, x, leaf).insert(x.next))
```
