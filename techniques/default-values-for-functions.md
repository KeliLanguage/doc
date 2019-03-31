# Default values for functions

By default, Keli does not supports default values for function parameters. However, this can be emulated using objects and lambda's shorthand. Not only that, it will be much better than the default values in other languages such as Python and TypeScript in terms of code completion supports. 

The following demonstrates how to create a graph library with a plot function that takes default values.

```c
// Defining structure of a point
Point = $.x(Float) y(Float)


// Defining default graph config
GraphType = choice.Line.Scatter.Bar
GraphConfig = $.
    type(GraphType) 
    xAxisLabel(String) 
    yAxisLabel(String) 
    title(String)
    
// defining default graph config
defaultGraphConfig = GraphConfig.
    type(GraphType.Line) 
    xAxisLabel("x") 
    yAxisLabel("y")
    title("title")

// defining the plot function
(this Array.of(Point)).plot(f Function.in(GraphConfig) out(GraphConfig)) = 
    this._plot(f.apply(defaultGraphConfig)) // the magic happens here

// defining the plot function helper
(this Array.of(Point))._plot(config GraphConfig) = undefined

// usage
points = [$.x(0) y(0), $.x(1) y(1), $.x(2) y(2)]
points.plot(.title("My graph").xAxisLabel(""))
```

