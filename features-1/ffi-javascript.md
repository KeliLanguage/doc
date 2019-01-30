---
description: >-
  This page shall describe how can JavaScript be embedded within Keli using
  Foreign Function Interface (FFI).
---

# FFI JavaScript

JavaScript can be embedded in Keli using the magic function `ffi.javascript` . For example,

```python
(this int).sqrt | int = (ffi.javascript "Math.sqrt(_this)") : int;
```

{% hint style="info" %}
Note

Every function parameter that is to be used in the embedded JS must be prefix-ed with one underscore.

Also, each `ff.javascript` expressions must be annotated with the expected types. 
{% endhint %}

{% hint style="danger" %}
Warning

Using FFI may break the soundness of type checking of Keli if the function parameter's type or return type is declared wrongly.
{% endhint %}

