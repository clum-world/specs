# Clum言語のControls

Controlsは、プログラムの実行フローを管理する組み込み言語構造。
通常の関数として実装することはできず、importも不要。

## Controlのカテゴリ

### 同期フロー制御

#### if

条件を評価し、2つの分岐のいずれかを実行する条件制御。

```
// 基本構文
result = if (condition)
  then -> expression1
else
  expression2

// killerあり
value = if (condition) (killer: timeout (scope: parent))
  then -> process-true
else
  process-false
```

#### loop

コードを繰り返し実行する反復制御。

```
// 基本構文
results = loop (collection)
  item -> process item

// killerあり
processed = loop (items) (killer: done (scope: current))
  item -> transform item
```

### 並行フロー制御

#### fork
並列処理のための新しい実行コンテキストを作成。

```
handle = fork (process)
handle = fork (killer: timeout (scope: current))
  heavy-computation
```

#### join

forkされたプロセスの完了を待機。

```
result = handle |> join
```

#### kill

forkされたプロセスを終了。

```
handle |> kill
```

#### race

複数のプロセスから最初に完了した結果を返す。

```
result = race [handle1, handle2, handle3]
```

#### take-until

条件が満たされるまで実行。

```
result = process |> take-until condition
```

### 通信制御

#### channel

プロセス間の通信チャネルを作成。

```
ch = channel i32
```

#### send

チャネルを通じてデータを送信。

```
ch |> send value
```

#### receive

チャネルからデータを受信。

```
value = ch |> receive
```

## Controlの特性

### スコープの作成

すべてのcontrolは新しい実行スコープを作成します：

```
outer (killer: k1 (scope: all))
  inner (killer: k2 (scope: parent))
    // 処理
```

### Killerシステム

各controlはkiller条件を設定でき、実行を制御できます：

```
loop (killer: timeout (scope: current))
  // timeoutが発生したらこのloopだけ終了
```

### 式としての評価

すべてのcontrolは値を返す式として機能します：

```
result = if (x > 0) then positive else negative
mapped = loop (items) item -> item * 2
```

## スコープレベル

killerのスコープは以下のレベルを指定できます：

- `current` - 現在のcontrolのみ終了
- `parent` - 親スコープまで終了
- `all` - すべてのスコープを終了
- `level(n)` - n階層上まで終了
- `named("label")` - 指定されたラベルまで終了

## 実装の注意点

Controlsは言語の実行モデルに深く関わるため：

1. コンパイラによる特別な処理が必要
2. 他の関数では実装不可能
3. 最適化の重要な対象
4. importなしで常に利用可能
