# Generic tagged union

## Generic tagged unions

The famous Maybe monad can be defined as such:

```haskell
(A Type).maybe | Type = (tag.nothing).or(tag.just(A))
```

