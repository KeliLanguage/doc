# Sample programs

## Fibonacci sequence

```c
// Definition
(n Int).fibonacci | Int = 
    n.==(0).or(n.==(1)).
        if(true):  
            (n)
        if(false):
            (n.-(1).fibonacci.+(n.-(2).fibonacci))

// Usage
= 10.fibonacci
```

## List operations

```c
="Definition of map"
{A Type} {B Type}
(this List.of(A)).map(f Function.in(A) out(B)) | List.of(B) =
    this.
        if(nil):  
            (List.nil)
        if(cons.as(c)):
            (c.current(x | f.apply(x)).next(.map(f)))

="Usage of map"
=[1 2 3 4 5].map(.+(2))
```

## Binary tree

```java
="Definition of binary tree"
BinaryTree.of(A Type)
    =  (tag.leaf)
    .or(tag.node(
            object.
                current(A) 
                left   (BinaryTree.of(A)) 
                right  (BinaryTree.of(A))))

="Definition of preorder function"
{A Comparable}
(this BinaryTree.of(A)).toPreorder | List.of(A) =
    this.
        leaf? 
            (List.nil)

        node(x)? 
            (List.nil.add(t.carry.current)
            .++(x.left.toPreorder)
            .++(x.right.toPreorder))

="Construction of a sample binary tree"
sampleTree 
    = List.node(object.
        current (1)
        left    (List.leaf)
        right  (List.node(object.
            current (2)
            left    (List.leaf)
            right   (List.leaf)
         )))

 =console.log(sampleTree)
 =console.log(sampleTree.toPreorder)
```

