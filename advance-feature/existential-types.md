# Existential types

## Example

Suppose we want to build a simple software that allows user to draw different shapes and to calculate their total areas.

To model this scenario, we can use the following code:

```text
radius = type.alias float;
side = type.alias float;

shape 
    =  (tag.# circle carry radius)
    .or(tag.# square carry side);

this:shape.area | float = 
    this.
        circle? (pi .* (this.carry.square))
        square? (this.carry.square);
```

No doubt, the code above will work fine. However, if we wish to allow external developers to define new shapes without modifying the base code, it won't be possible.

To achieve **extensibility**, we need to use _existential types._ The following code is what happened when we use _existential types_.

```text
gotArea = interface;

{a:gotArea}
this:a.area | float = toBeDefined;

{a:gotArea} 
shape = (tag.# newShape carry a);

radius = type.alias float;
side = type.alias float;

circle = (tag.# newCircle carry radius);
this:circle.area | float = circle.newCircle? (pi .* (this.carry.square));

square = (tag.# newSquare carry side);
this:square.area | float = this.newSquare? (this.carry.square);
```

## Reference

{% embed url="https://wiki.haskell.org/Existential\_type" caption="" %}

