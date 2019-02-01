# Generic tagged union

## Generic tagged unions

The famous Maybe monad can be defined as such:

```text
a:type.maybe | type = (tag.# nothing).or(tag.# just carry a)
```

