## Functions

```js
// unary function
a:string, reverse -> string = 

a:integer, ! -> integer = 
    a, <= 0, ifTrue 1 ifFalse (a, * (a, - 1, !))

// binary function
a:string, spiltBy y:string -> string = 

// ternary function
a:string, replace old:string with new:string -> string 
    = a, splitBy old, joinBy new
```