# Sample programs

## Fibonacci sequence

```javascript
="Definition"
(n Int).fibonacci | Int = 
    n.==(0).or(n.==(1)).
        true?  (n)
        false? ((n.-(1)).fibonacci.+((n.-(2)).fibonacci)))

="Usage"
=console.log(10.fibonacci)
```

## List operations

```bash
="Definition of map"
{A Type} {B Type}
(this List.of(A)).map(f Function.from(A) to(B)) | List.of(B) =
    this.
        nil?  (List.nil)
        cons? (f.apply(this.current).cons(xs.next.map(f)))

="Usage of map"
=[1 2 3 4 5].map(x|x.+(2))
```

## Binary tree

```java
="Definition of binary tree"
BinaryTree.of(A Type)
    =  (tag.leaf)
    .or(tag.node(
            record.
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
    = List.node(record.
        current (1)
        left    (List.leaf)
        right  (List.node(record.
            current (2)
            left    (List.leaf)
            right   (List.leaf)
         )))

 =console.log(sampleTree)
 =console.log(sampleTree.toPreorder)
```

