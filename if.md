# if

条件分岐

```clum
if
  a gt 100: full!
  a gt 90: 90-percent!
  a gt 80: 80-percent!
  otherwise: fn!
```

複数条件

```clum
if
  a gt 100 and b is true: full!
  otherwise: fn!
```

改行パターン

```clum
if
  a gt 100 and b is true:
    full!
  otherwise:
    fn!
```

一行で書く場合はfallbackは指定できない。

```clum
if a gt 100: full!
```

早期return

```clum
if a gt 100: <- full!
```

型パターンマッチ

```clum
if
  answer is Yes: exit(answer.type)! // answerがokに指定された型になっている
  answer is No: crash(answer.cause)! // answerがngに指定された型になっている
```

if式

```clum
r = if
  a gt 100: Yes(100)!
  fallback: No(message)!

r = if
  answer is Yes: 'ok'
  answer is No: 'ng'
```

else, elseif は実装しない。

```clum
if a gt 100
  full!
elseif a gt 90
  90-percent!
elseif a gt 80
  80-percent!
else
  fn!
```

otherwiseにした理由はfallbackもdefaultも変数、関数で使用したいから。

```clum
# Dispatch (type: Type, fallback: Action) -> Action
# Configure (config: Omit<Config>, default: Config) -> Config

if
  connection is-active: use-primary!
  connection is-idle: reconnect!
  otherwise: use-fallback!  // fallbackを変数として使える

# LoadConfig (path: String, fallback: Config) -> Config
load-config: LoadConfig (path, fallback) ->
  if
    file-exists: parse-file!
    otherwise: fallback  // パラメータ名として自然に使える
```
