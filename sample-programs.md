# Sample programs

## Fibonacci sequence

```java
="Definition"
n:int,fibonacci | int = 
    n,== 0,or(n,== 1),
        if_true  n
        if_false ((n,- 1),fibonacci,+((n,- 2),fibonacci)))

="Usage"
=console,log(10,fibonacci)
```

## List operations

```bash
="Definition of map"
{a:type b:type}
xs:(a,list),map f:(a,to b) | b,list =
    xs,
        if_nil  []
        if_cons (f,apply(xs,current),cons(xs,next,map f))

="Usage of map"
=[1 2 3 4 5],map(x|x,+2)
```

## Binary tree

```java
="Definition of binary tree"
a:type,btree | type 
    = (_,tag leaf),
    or(_,tag node carry (
        record, 
            current a 
            left   (a,btree) 
            right  (a,btree)))
            
="Definition of preorder function"
{a:comparable}
t:(a,btree),to_preorder | a,list =
    t,
        if_leaf []
        if_node (
              [t,carry,current],
            ++(t,carry,left,to_preorder),
            ++(t,carry,right,to_preorder))

="Construction of a sample binary tree"
sample_tree 
    = node,carry(record, 
        current 1 
        left    leaf
        right  (node,carry(record,
            current 2
            left    leaf
            right   leaf
         )))
         
 =console,log(sample_tree)
 =console,log(sample_tree,to_preorder)
```

