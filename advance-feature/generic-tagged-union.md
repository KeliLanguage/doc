# Generic tagged union



## Generic tagged unions

The famous Maybe monad can be defined as such:

```text
a:type.maybe | type = (_.tag nothing).or(_.tag just carry a)
```

