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
    kind "people" 
    fields [
        ("name"   string)
        ("age"    integer)
        ("friend" people.maybe)
    ]

// construction
x = data.new "people" values [ ("name" "wong") ("age" 9) ("friend" nil) ]

// access
console.log(x.get "name".toUpperCase)

// update
y = x
    .set "name" as "lee"
    .set "age"  as 9
console.log(y) // {name: "wong", }
```

## Binary Tree example

```smalltalk
a:type.btree -> type = 
    (data.kind "leaf")
    .or
    (data
        .kind "node"
        fields [
            ("left"    a.btree)
            ("current" a)
            ("right"   a.btree)
        ]
    )

{a:comparable} 
(tree:a.btree).insert value:a -> a.btree = 
    tree.kind.case [
        ("leaf" 
            (node.
                left    leaf 
                current value 
                right   leaf))
        ("node" 
            (value.< node.value.
                ifYes (tree.left  (tree.left.insert value))
                ifNot (tree.right (tree.right.insert value))))
    ]

```