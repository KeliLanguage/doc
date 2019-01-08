# Sample programs

## Fibonacci sequence

```javascript
="Definition"
n:int.fibonacci | int = 
    n.== 0.or(n.== 1).
        true?  n
        false? ((n.- 1).fibonacci.+((n.- 2).fibonacci)))

="Usage"
=console.log(10.fibonacci)
```

## List operations

```bash
="Definition of map"
{a:type b:type}
xs:(a.list).map f:(a.to b) | b.list =
    xs.
        nil?  []
        cons? (f.apply(xs.current).cons(xs.next.map f))

="Usage of map"
=[1 2 3 4 5].map(x|x.+2)
```

## Binary tree

```java
="Definition of binary tree"
a:type.btree | type 
    =  (_.tag leaf)
    .or(_.tag node carry (
            record.
                current a 
                left   (a.btree) 
                right  (a.btree)))
            
="Definition of preorder function"
{a:comparable}
t:(a.btree).toPreorder | a.list =
    t.
        leaf? []
        node? ([t.carry.current]
            .++(t.carry.left.toPreorder)
            .++(t.carry.right.toPreorder))

="Construction of a sample binary tree"
sampleTree 
    = node.carry(record.
        current 1 
        left    leaf
        right  (node.carry(record.
            current 2
            left    leaf
            right   leaf
         )))
         
 =console.log(sampleTree)
 =console.log(sampleTree.toPreorder)
```

