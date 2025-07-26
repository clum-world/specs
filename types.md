# Primitives

## 該当の型

- i8
- i16
- i32
- i64
- i128
- u8
- u16
- u32
- u64
- u128
- f32
- f64

## 分類条件

### 1. 分解不可能性

これ以上小さな部品に分解できない

- 42: i32 - これ以上分解できない
- 3.14: f64 - これ以上分解できない

### 2. 言語組み込み

言語が直接提供する基本型

- ユーザーが定義できない
- コンパイラが特別に認識

### 3. メモリ上の表現が固定

決まったビット数で表現

- i8 = 8ビット
- i32 = 32ビット  
- f64 = 64ビット

### 4. 他の型に依存しない

- i32 - 独立している
- Array T - Tに依存するのでPrimitiveではない
- Option T - Tに依存するのでPrimitiveではない

### 5. 演算が直接定義される

- 1 + 2 - CPUの加算命令に直接対応
- 3.14 * 2.0 - CPUの乗算命令に直接対応

これらすべての条件を満たすものがPrimitive型となる。

# Variants

## 該当の型

- true
- false
- Some T
- None
- Ok T
- Err E

## 分類条件

### 1. 値コンストラクタである

特定の型の値を生成するコンストラクタとして機能する

- true - Bool型の値を生成
- Some 42 - Option i32型の値を生成
- Ok 'success' - Result String Error型の値を生成

### 2. 選択肢の一つを表現

複数の可能性のうち一つを表す

- true | false - 2つの選択肢
- Some T | None - 値があるかないか
- Ok T | Err E - 成功か失敗か

### 3. パターンマッチングの対象

条件分岐で直接比較される

- value is Some - Someバリアントかチェック
- result is Ok - Okバリアントかチェック
- flag is true - trueバリアントかチェック

### 4. 単体では型ではない

- Some - 型ではなくOption型のコンストラクタ
- true - 型ではなくBool型の値
- Ok - 型ではなくResult型のコンストラクタ

### 5. Composite型の構成要素

Variantを組み合わせてComposite型が作られる

- Bool = true | false
- Option T = Some T | None
- Result T E = Ok T | Err E

これらすべての条件を満たすものがVariantとなる。

# Composites

## 該当の型

- Bool
- String
- Array T
- Set T
- Map K V
- Option T
- Result T E
- Tuple (T1, T2, ...)
- Struct（ユーザー定義構造体）

## 分類条件

### 1. 複数の要素から構成される

他の型やVariantを組み合わせて作られる

- Bool = true | false の合成
- String = [u8] - バイトの配列
- Option T = Some T | None の合成
- Tuple (i32, String) - 複数の型の組み合わせ

### 2. 分解可能である

構成要素に分解できる

- String 'hello' - 個々のバイト [104, 101, 108, 108, 111] に分解
- Option (Some 42) - Some と 42 に分解
- Tuple (10, 'text') - 10 と 'text' に分解

### 3. 型パラメータを持つことができる

ジェネリクスとして他の型を受け取れる

- Array T - 任意の型Tの配列
- Option T - 任意の型Tのオプション
- Result T E - 成功型Tと失敗型E
- Map K V - キー型Kと値型V

### 4. 制約や規則を持つ

単純な組み合わせ以上の意味を持つ

- Bool - 0 or 1のみの制約
- String - UTF-8エンコーディングの制約
- Set T - 重複なしの制約
- Map K V - キーの一意性の制約

### 5. 高レベルの抽象化を提供

Primitiveより高次の概念を表現

- Option T - 値の有無という概念
- Result T E - 成功/失敗という概念
- String - テキストという概念
- Map K V - キー値対応という概念

これらの条件を満たすものがComposite型となる。
