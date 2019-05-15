<!--
# PureScript Language Reference
-->
# PureScript言語リファレンス

<!--
As an introductory example, here is the usual "Hello World" written in PureScript:
-->
例として、PureScriptの「Hello World」をお見せします：

```purescript
module Main where

import Effect.Console

main = log "Hello, World!"
```

<!--
## Another Example
-->
## 別の例

<!--
The following code defines a `Person` data type and a function to generate a string representation for a `Person`:
-->
以下のコードは`Person`データ型と`Person`の文字列表現を生成する関数を定義します：

```purescript
data Person = Person { name :: String, age :: Int }

showPerson :: Person -> String
showPerson (Person o) = o.name <> ", aged " <> show o.age

examplePerson :: Person
examplePerson = Person { name: "Bonnie", age: 26 }
```

<!--
Line by line, this reads as follows:
-->
各行は以下のように読むことができます：

<!--
- `Person` is a data type with one constructor, also called `Person`
- The `Person` constructor takes an object with two properties, `name` which is a `String`, and `age` which is an `Int`
- The `showPerson` function takes a `Person` and returns a `String`
- `showPerson` works by case analysis on its argument, first matching the constructor `Person` and then using string concatenation and object accessors to return its result.
- `examplePerson` is a Person object, made with the `Person` constructor and given the String "Bonnie" for the name value and the Int 26 for the age value.
-->
- `Person`は1つのコンストラクタを伴うデータ型であり、`Person`とも呼ばれる。
- `Person`コンストラクタは2つのプロパティを持つオブジェクトを1つ受け取る。各プロパティは、`String`型の`name`と`Int`型の`age`である。
- `showPerson`関数は`Person`を受け取り、`String`を返す。
- `showPerson`関数は、引数の解析によって実現される。まずは`Person`コンストラクタにマッチし、その後にオブジェクトのアクセサと文字列結合の結果を返す。
- `examplePerson`はPersonオブジェクトであり、`Person`コンストラクタに、nameの値としての文字列"Bonnie"とageの値としての整数26を与えることで作成される。

言語リファレンスの全容は以下に続きます:

1. [型](Types.md)
2. [構文](Syntax.md)
3. [型クラス](Type-Classes.md)
4. [パターンマッチ](Pattern-Matching.md)
5. [モジュール](Modules.md)
6. [外部関数インタフェース(FFI)](FFI.md)
7. [レコード](Records.md)
8. [Haskellとの違い](Differences-from-Haskell.md)
