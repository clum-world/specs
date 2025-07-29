# loop

何も指定しないなら無限ループ。

```clum
loop
  fn!
```

指定回数をloop

```clum
loop (count: 10)
  fn!
```

index付き

```clum
loop (count: 10) with (...index)
  fn(index)!
```

```clum
loop (
  count: 10
  killer:
    from-index index -> index > 2
    or abort-signal
    or timeout(200ms)
) with (...index, abort-signal)
  fn(index)!
  if a is true: abort-signal!
```