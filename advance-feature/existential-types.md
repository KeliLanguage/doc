# Existential types

## Example

Suppose we want to build a simple software that allows user to draw different shapes and to calculate their total areas.

To model this scenario, we can use the following code:

```haskell
Radius = Type.alias(Float)
Side = Type.alias(Float)

Shape
    =  (tag.#(circle) carry(Radius))
    .or(tag.#(square) carry(Side))

(this Shape).area | Float = 
    this.
        circle? (pi.*(this.carry.square))
        square? (this.carry.square)
```

No doubt, the code above will work fine. However, if we wish to allow external developers to define new shapes without modifying the base code, it won't be possible.

To achieve **extensibility**, we need to use _existential types._ The following code is what happened when we use _existential types_.

```haskell
GotArea = interface

{A GotArea}
(this A).area | Float = tobedefined

{A GotArea} 
Shape = tag.#(newShape) carry(A)

Radius = Type.alias(Float)
Side = Type.alias(Float)

Circle = tag.#(newCircle) carry(Radius)
(this Circle).area | Float = this.newCircle? (pi .* (this.carry.square))

Square = tag.#(newSquare) carry(Side)
(this Square).area | Float = this.newSquare? (this.carry.square)
```

## Reference

{% embed url="https://wiki.haskell.org/Existential\_type" caption="" %}

