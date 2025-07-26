# 演算子

Clumの演算子は7つのみ。その他の操作はすべて関数として提供される。

- `#`
  - 型や関数を宣言する際に使用
- `|>`
  - パイプライン演算子
  - データフロー
- `!`
  - 実行演算子
- `()`
  - グループ化
  - 優先順位の明示
  - 配列など集合の表現
- `->`
  - 関数定義
- `:`
  - 実装
  - 型と実装の結合
- `=`
  - 束縛
  - a = 1
  - fn = (a, b) -> a add b

## 関数として提供される演算

```clum
r = 1 |> add 1 |> ! // 加算
r = 1 |> sub 1 |> ! // 減算
r = 1 |> mul 1 |> ! // 乗算
r = 1 |> div 1 |> ! // 除算
```

## 比較演算

```clum
a |> gt b // >
a |> gte b // >=
a |> lt b // <
a |> lte b // <=
a |> is b // ==
a |> in x, y // 包含
```

## 論理演算

```clum
a |> and b // 論理積
a |> or b // 論理和
a |> not // 否定（Bool型のみ）
```

## スペース規則

すべての演算子の前後にスペースが必須。

```clum
// OK
data |> process |> !

// ERROR
data|>process|>!
```

# コメント

行コメントのみサポートする。

## 構文

// 行コメント

## 使用例

```clum
// 単一行コメント
data |> process |> !

// 複数行にわたるコメント
// この関数は入力を検証し、
// 結果を保存する
validate-and-save: ValidateAndSave = (input) ->
  input |> validate |> save |> !

# Add (a: i32, b: i32) -> i64  // 型定義の後ろにもコメント可能
add: Add = (a, b) -> a + b       // 実装の後ろにもコメント可能
```

## 制約

- ブロックコメント（/* */）はサポートしない
- コメントは実行時に完全に無視される

# モジュールシステム

## import

```clum
// URLベースのimport
@https://registry.clum.world/std
  print
  print as p // 別名も可能

// ローカルファイルのimport  
@./utils/validation
  validate-email
  check-age

// config.clumにpath aliasを設定していればこのように書くことも可能
@~/path/to/file
  fn1
  fn2

// 使用例
email |> validate-email |> if valid then save else error |> !
```

```clum
// パスエイリアス（config.clumで定義）
# (pub) path-aliases: PathAliases = (
  '$utils': './src/utils'
  '$components': './src/components'
  '$lib': './external/lib'
)

@$utils/validation
  validate-email
```

## export

```
// publicな定義（exportされる）
# (pub) process-user (user: User) -> Result

// ディレクトリスコープ（同じディレクトリ内でのみ参照可能）
# (dir) internal-helper (data: Data) -> Data

// デフォルトはprivate（exportされない）
# helper-function () -> Unit
```
