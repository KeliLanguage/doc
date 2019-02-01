# FFI JavaScript

JavaScript can be embedded in Keli using the magic function `ffi.javascript` . For example,

```haskell
(this Int).sqrt | Int = ffi.javascript("Math.sqrt(_this)").as(Int)
```

{% hint style="info" %}
Note

Every function parameter that is to be used in the embedded JavaScript must be prefix-ed with one underscore.

Also, each `ffi.javascript` expressions must be annotated with the expected types using the `.as` function. 
{% endhint %}

{% hint style="danger" %}
Warning

Using FFI may break the soundness of type checking of Keli if the function parameter's type or return type is declared wrongly.
{% endhint %}

