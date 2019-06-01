<!--
## Evaluation strategy
-->
## 評価戦略

<!--
Unlike Haskell, PureScript is strictly evaluated.
-->
Haskellとは異なり、PureScriptは正格評価です。

<!--
As the evaluation strategy matches JavaScript, interoperability with existing code is trivial - a function exported from a PureScript module behaves exactly like any "normal" JavaScript function, and correspondingly, calling JavaScript via the FFI is simple too.
-->
JavaScriptに合わせた評価戦略になっています。既存のコードとの相互運用性は些細なものであり、PureScriptのモジュールから生成された関数はJavaScriptの「普通の」関数と正確に同じように振る舞います。同様に、FFIを通じてJavaScriptを呼び出すことも単純です。

<!--
Keeping strict evaluation also means there is no need for a runtime system or overly complicated JavaScript output. It should also be possible to write higher performance code when needed, as introducing laziness on top of JavaScript comes with an unavoidable overhead.
-->
正格評価を保つことは、実行時システムや過度に複雑なJavaScriptコードを必要としないことも意味します。必要なときには高いパフォーマンスのコードを書くこともできるべきであり、JavaScriptの上に遅延を導入することは避けられないオーバーヘッドを招くことになります。

<!--
## Prelude/base
-->
## Prelude／base

<!--
There is no implicit `Prelude` import in PureScript, the `Prelude` module is just like any other. Also, no libraries are distributed with the compiler at all.
-->
PureScriptでは`Prelude`を他のライブラリと同様に扱い、暗黙的なインポートを行いません。また、コンパイラと共に配布されるライブラリはありません。

<!--
The generally accepted "standard" `Prelude` is the [`purescript-prelude`](https://github.com/purescript/purescript-prelude) library.
-->
一般に受け入れられている「標準の」`Prelude`ライブラリは[`purescript-prelude`](https://github.com/purescript/purescript-prelude)です。

<!--
## Module Imports / Exports
-->
## モジュールのインポート／公開

<!--
Type classes in modules must be specifically imported using the `class` keyword.
-->
モジュール内の型クラスは`class`キーワードと共に明示的にインポートされる必要があります。

```purescript
module B where

import A (class Fab)
```

<!--
There is no `qualified` keyword in PureScript. Writing `import Data.List as List` has the same effect as writing `import qualified Data.List as List` in Haskell.
-->
PureScriptには`qualified`キーワードがありません。単に`import Data.List as List`と書くことで、Haskellの`import qualified Data.List as List`と同様の効果を持ちます。

<!--
Module imports and exports are fully documented on the [Modules](Modules.md) page.
-->
モジュールのインポートと公開については[Modules](Modules.md)で完全にドキュメント化されています。

<!--
## Types
-->
## 型

<!--
### Explicit forall
-->
### 明示的な全称量化

<!--
Polymorphic functions in PureScript require an explicit `forall` to declare type variables before using them. For example, Haskell's list `length` function is declared like this:
-->
PureScriptの多相関数は、型変数の使用前宣言のために明示的な`forall`を必要とします。例えば、Haskellのリストの`length`関数は次のように宣言されています：

``` haskell
length :: [a] -> Int
```

<!--
In PureScript this will fail with the error `Type variable a is undefined`. The PureScript equivalent is:
-->
PureScriptでは、これは`Type variable a is undefined`エラーによって失敗するため、次のように書く必要があります：

``` purescript
length :: forall a. Array a -> Int
```

<!--
A `forall` can declare multiple type variables at once, and should appear before typeclass constraints:
-->
`forall`は複数の型変数を一度に宣言することができます。また、型クラス制約の前に記述する必要があります：

``` purescript
ap :: forall m a b. (Monad m) => m (a -> b) -> m a -> m b
```

<!--
### Numbers
-->
### 数値

<!--
There is a native `Number` type which represents JavaScript's standard IEEE 754 float and an `Int` which is restricted to the range of 32-bit integers. In JavaScript, the `Int` values and operations are generated with a `|0` postfix to achieve this, e.g. if you have variables `x`, `y`, and `z` of type `Int`, then the PureScript expression `(x + y) * z` would compile to `((x + y)|0 * z)|0`.
-->
JavaScriptの標準であるIEEE 754単精度浮動小数点数を表す`Number`型と、32ビットに制限された整数の`Int`があります。JavaScriptでは、これを達成するために`Int`値と演算子は接尾辞`|0`によって生成されます。例えば、`Int`型の変数`x`、`y`、`z`があるとき、PureScriptの式`(x + y) * z`は`((x + y)|0 * z)|0`にコンパイルされます。

<!--
### Unit
-->
### ユニット

<!--
PureScript has a type `Unit` used in place of Haskell's `()`. The `Prelude` module provides a value `unit` that inhabits this type.
-->
PureScriptにはHaskellの`()`に対応する`Unit`型があります。`Prelude`モジュールがその型を持つ値`unit`を提供しています

<!--
### `[a]`
-->
### `[a]`

<!--
PureScript does not provide syntactic sugar for list types. Construct list types using `List` from `Data.List`.
-->
PureScriptはリスト型の糖衣構文を提供しません。リストは`Data.List`にある`List`を用いて生成されます。

<!--
There is also an `Array` type for native JavaScript arrays, but this does not have the same performance characteristics as `List`. `Array` _values_ can be constructed with `[x, y, z]` literals, but the type still needs to be annotated as `Array a`.
-->
JavaScriptの配列のために`Array`型もありますが、これは`List`と同じパフォーマンス特性を持ちません。`Array`*値*は`[x, y, z]`リテラルとして生成できますが、型の場合は注釈として`Array a`が必要になります。

<!--
## Records
-->
## レコード

<!--
PureScript can encode JavaScript-style objects directly by using row types, so Haskell-style record definitions actually have quite a different meaning in PureScript:
-->
PureScriptは列型を用いてJavaScriptスタイルのオブジェクトを直接エンコードできるので、Haskellスタイルのレコード定義が全く異なる意味を持ちます。

``` purescript
data Point = Point { x :: Number, y :: Number }
```

<!--
In Haskell a definition like this would introduce several things to the current environment:
-->
このようなHaskellの定義は現在の環境にいくつかのことを導入します：

``` haskell
Point :: Number -> Number -> Point
x :: Point -> Number
y :: Point -> Number
```

<!!--
However in PureScript this only introduces a `Point` constructor that accepts an object type. In fact, often we might not need a data constructor at all when using object types:
-->
しかしPureScriptで導入されるのは、オブジェクト型を受け入れる`Point`コンストラクタだけです。実際、オブジェクト型を使うときにはデータコンストラクタを必要としないことがよくあります：

``` purescript
type PointRec = { x :: Number, y :: Number }
```

<!--
Objects are constructed with syntax similar to that of JavaScript (and the type definition):
-->
オブジェクトとオブジェクト型の定義はJavaScriptのそれと似た構文で構成されます：

``` purescript
origin :: PointRec 
origin = { x: 0, y: 0 }
```

<!--
And instead of introducing `x` and `y` accessor functions, `x` and `y` can be read like JavaScript properties:
-->
また、`x`、`y`のアクセサ関数を導入する代わりに、`x`、`y`はJavaScriptのプロパティのようにアクセスできます：

``` purescript
originX :: Number
originX  = origin.x
```

<!--
PureScript also provides a record update syntax similar to Haskell's:
-->
PureScriptはHaskellと似たレコード更新の構文も提供します：

``` purescript
setX :: Number -> PointRec -> PointRec 
setX val point = point { x = val }
```

<!--
A common mistake to look out for is when writing a function that accepts a data type like the original `Point` above—the object is still wrapped inside `Point`, so something like this will fail:
-->
気を付けるべきよくある間違いは、上記の`Point`のようなデータ型を受け入れる関数を書くときに起こります。そのオブジェクトはまだ`Point`の内側にあるので、次のようにすると失敗します：

``` purescript
showPoint :: Point -> String
showPoint p = show p.x <> ", " <> show p.y
```

<!--
Instead, we need to destructure `Point` to get at the object:
-->
代わりに、`Point`型を分解してオブジェクトを得るようにします：

``` purescript
showPoint :: Point -> String
showPoint (Point obj) = show obj.x <> ", " <> show obj.y
```

<!--
## Type classes
-->
## 型クラス

<!--
### Arrow direction
-->
### 矢印の向き

<!--
When declaring a type class with a superclass, the arrow is the other way around. For example:
-->
スーパークラスと共に型クラスを宣言している時、矢印は逆向きになります。例えば：

```purescript
class (Eq a) <= Ord a where
  ...
```

<!--
This is so that `=>` can always be read as logical implication; in the above case, an `Ord a` instance _implies_ an `Eq a` instance.
-->
これは`=>`を常に論理包含として読むことができるようにするためで、上記の場合、`Ord a`インスタンスは`Eq a`インスタンスを*意味します*。

<!--
### Named instances
-->
### 名前付きインスタンス

<!--
In PureScript, instances must be given names:
-->
PureScriptでは、インスタンスは名前を持つ必要があります：

```purescript
instance arbitraryUnit :: Arbitrary Unit where
  ...
```

<!--
Overlapping instances are still disallowed, like in Haskell. The instance names are used to help the readability of compiled JavaScript.
-->
Haskellのようにインスタンスの重複は許されていません。インスタンス名はコンパイル後のJavaScriptの可読性を向上するために使用されます。

<!--
### Deriving
-->
### 導出

<!--
Unlike Haskell, PureScript doesn't have deriving functionality when declaring
data types.  For example, the following code does not work in PureScript:
-->
Haskellとは異なり、PureScriptはデータ型の宣言時に導出を行うことができません。例えば、次のようなコードは動きません：

```haskell
data Foo = Foo Int String deriving (Eq, Ord)
```

<!--
However, PureScript does have `StandaloneDeriving`-type functionality:
-->
しかし、PureScriptは`StandaloneDeriving`機能を持ちます：

```purescript
data Foo = Foo Int String

derive instance eqFoo :: Eq Foo
derive instance ordFoo :: Ord Foo
```

<!--
Examples of type classes that can be derived this way include `Eq`, `Functor`,
and `Ord`.  See
[here](https://github.com/purescript/documentation/blob/master/language/Type-Classes.md#type-class-deriving)
for a list of other type classes.
-->
この方法で導出される型クラスの例は、`Eq`、`Functor`、`Ord`です。その他の型クラスの一覧は[こちら](https://github.com/purescript/documentation/blob/master/language/Type-Classes.md#type-class-deriving)を参照してください。

<!--
Using generics, it is also possible to use generic implementations for type
classes like `Bounded`, `Monoid`, and `Show`.  See
[the generics-rep library](https://pursuit.purescript.org/packages/purescript-generics-rep)
for a list of other type classes that have generic implementations, as well as
an explanation of how to write generic implementations for your own type
classes.
-->
`Bounded`、`Monoid`、`Show`のような型クラスのために、総称を使って一般的な実装を行うことも可能です。一般的な実装を持つ他の型クラスの一覧は[generics-repライブラリ](https://pursuit.purescript.org/packages/purescript-generics-rep)を参照してください。一般的な実装を持つ型クラスを書く方法も説明されています。

<!--
### Orphan Instances
-->
### 孤児インスタンス

<!--
Unlike Haskell, orphan instances are completely disallowed in PureScript.  It is a compiler error to try to declare orphan instances.
-->
Haskellとは異なり、PureScriptでは孤児インスタンスを完全に禁止しています。孤児インスタンスを宣言しようとするとコンパイラエラーになります。

<!--
When instances cannot be declared in the same module, one way to work around it is to use [newtype wrappers](http://stackoverflow.com/questions/22080564/whats-the-practical-value-of-all-those-newtype-wrappers-in-data-monoid).
-->
インスタンスが同一モジュール内で宣言できない場合、これを回避する方法の一つは[ユーザ定義型ラッパ](http://stackoverflow.com/questions/22080564/whats-the-practical-value-of-all-those-newtype-wrappers-in-data-monoid)を使用することです。

<!--
### Default members
-->
### デフォルトメンバ

<!--
At the moment, it is not possible to declare default member implementations for type classes. This may change in the future.
-->
現在、型クラスのデフォルトメンバ実装を宣言することはできません。今後可能になるかもしれません。

<!--
### Type class hierarchies
-->
### 型クラスの階層

<!--
Many type class hierarchies are more granular than in Haskell. For example:
-->
多くの型クラスの階層はHaskellよりも細かくなっています。例えば：

<!--
* `Category` has a superclass `Semigroupoid` which provides `(<<<)`, and does not require an identity.
* `Monoid` has a superclass `Semigroup`, which provides `(<>)`, and does not require an identity.
* `Applicative` has a superclass `Apply`, which provides `(<*>)` and does not require an implementation for `pure`.
-->
* `Category`は`(<<<)`を提供するスーパークラス`Semigroupoid`を持ち、恒等関数を必要としません。
* `Monoid`は`(<>)`を提供するスーパークラス`Semigroup`を持ち、恒等関数を必要としません。
* `Applicative`は`(<*>)`を提供するスーパークラス`Apply`を持ち、`pure`の実装を必要としません。

<!--
## Tuples
-->
## タプル(組)

<!--
PureScript has no special syntax for tuples as records can fulfill the same role that *n*-tuples do with the advantage of having more meaningful types and accessors.
-->
PureScriptはタプルのための特別な構文を持ちません。レコードがより意味のある型とアクセサを持つ利点によって*n*-組と同じ役割を果たすことができるためです。

<!--
A `Tuple` type for 2-tuples is available via the [purescript-tuples](https://github.com/purescript/purescript-tuples) library. `Tuple` is treated the same as any other type or data constructor.
-->
2-組の`Tuple`型は[purescript-tuples](https://github.com/purescript/purescript-tuples)ライブラリから使用可能です。`Tuple`は他の型やデータコンストラクタと同様に扱えます。

<!--
## Composition operator
-->
## 合成演算子

<!--
PureScript uses `<<<` rather than `.` for right-to-left composition of functions. This is to avoid a syntactic ambiguity with `.` being used for property access and name qualification. There is also a corresponding `>>>` operator for left-to-right composition.
-->
PureScripteでは右から左への関数合成のために`.`ではなく`<<<`を使います。これは`.`がプロパティへのアクセサや修飾名のために使われていることによる文法的な曖昧さを避けるためです。左から右への合成のために`>>>`もあります。

<!--
The `<<<` operator is actually a more general morphism composition operator that applies to semigroupoids and categories, and the `Prelude` module provides a `Semigroupoid` instance for the `->` type, which gives us function composition.
-->
`<<<`演算子は実際には半群や圏にも適用されるより一般的な射合成演算子です。`Prelude`モジュールは`->`型のために`Semigroupoid`インスタンスを提供しており、これが関数合成にあたります。

<!--
## `return`
-->
## `return`

<!--
In the past, PureScript used `return`. However, it is now removed and replaced with [`pure`](https://pursuit.purescript.org/packages/purescript-prelude/1.1.0/docs/Control.Applicative#v:pure). It was always an alias for `pure`, which means this change was implemented by simply removing the alias.
-->
PureScriptでは`return`を使っていた過去がありますが、現在は[`pure`](https://pursuit.purescript.org/packages/purescript-prelude/1.1.0/docs/Control.Applicative#v:pure)で置き換えられています。もともと`pure`の別名だったので、この置き換えは単に別名を削除したことで実装されました。

<!--
## Array Comprehensions
-->
## 配列内包表記

<!--
PureScript does not provide special syntax for array comprehensions. Instead, use `do`-notation. The `guard` function from the `Control.MonadPlus` module in `purescript-control` can be used to filter results:
-->
PureScriptは配列の内包表記に特別な構文を提供しません。代わりに`do`記法を使ってください。`purescript-control`にある`Control.MonadPlus`モジュール内の`guard`関数はフィルタ結果のために使われます：

```purescript
import Prelude (($), (*), (==), bind, pure)
import Data.Array ((..))
import Data.Tuple (Tuple(..))
import Control.MonadZero (guard)

factors :: Int -> Array (Tuple Int Int)
factors n = do
  a <- 1 .. n
  b <- 1 .. a
  guard $ a * b == n
  pure $ Tuple a b
```

<!--
## No special treatment of `$`
-->
## `$`の特別扱いがない

<!--
GHC provides a special typing rule for the `$` operator, so that the following natural application to the rank-2 `runST` function is well-typed:
-->
GHCは`$`演算子のための特別な型付け規則を提供するため、以下のランク2`runST`への自然な適用は関数を正しく型付けされています。 

```haskell
runST $ do
  ...
```

<!--
PureScript does not provide this rule, so it is necessary to either 
-->
PureScriptはこの規則を提供しないので、どちらかが必要です

<!--
- omit the operator: `runST do ...`
- or use parentheses instead: `runST (do ...)`
-->
- 演算子を省略する： `runST do ...`
- 代わりに丸括弧を使用する `runST (do ...)`

<!--
## Defining Operators
-->
## 演算子の定義

<!--
In Haskell, it is possible to define an operator with the following natural syntax:
-->
Haskellでは、次の自然な構文によって演算子の定義が可能です：

```haskell
f $ x = f x
```

<!--
In PureScript, you provide an operator alias for a named function. Defining functions using operators is removed since version 0.9.
-->
PureScriptでは、名前を持つ関数の別名として演算子を提供します。演算子を使用した関数の定義はバージョン0.9で削除されました。

```purescript
apply f x = f x
infixr 0 apply as $
```

<!--
## Operator Sections
-->
## 演算子セクション

<!--
In Haskell, there is syntactic sugar to partially apply infix operators.
-->
Haskellでは、中置演算子の部分適用のために糖衣構文があります。

```haskell
(2 ^) -- desugars to `(^) 2`, or `\x -> 2 ^ x`
(^ 2) -- desugars to `flip (^) 2`, or `\x -> x ^ 2`
```

<!--
In PureScript, operator sections look a little bit different.
-->
PureScriptでは、演算子セクションが少し異なります。

```purescript
(2 ^ _)
(_ ^ 2)
```

<!--
## Extensions
-->
## 拡張

<!--
The PureScript compiler does not support GHC-like language extensions. However, there are some "built-in" language features that are equivalent (or at least similar) to a number of GHC extensions. These currently are:
-->
PureScriptコンパイラはGHCのような言語拡張をサポートしません。しかし、GHC拡張と同様の（少なくとも似ている）いくつかの「組み込み」言語機能があります。現在は次のものがあります：

<!--
* DataKinds (see note below)
* EmptyDataDecls
* ExplicitForAll
* FlexibleContexts
* FlexibleInstances
* FunctionalDependencies
* KindSignatures
* MultiParamTypeClasses
* PartialTypeSignatures
* RankNTypes
* RebindableSyntax
* ScopedTypeVariables
-->
* DataKinds (以下の注意事項を見てください)
* EmptyDataDecls
* ExplicitForAll
* FlexibleContexts
* FlexibleInstances
* FunctionalDependencies
* KindSignatures
* MultiParamTypeClasses
* PartialTypeSignatures
* RankNTypes
* RebindableSyntax
* ScopedTypeVariables

<!--
Note on `DataKinds`: Unlike in Haskell, user-defined kinds are open, and they are not promoted, which means that their constructors can only be used in types, and not in values. For more information about the kind system, see https://github.com/purescript/documentation/blob/master/language/Types.md#kind-system
-->
※`DataKinds`の注意事項：Haskellとは異なり、ユーザ定義種は元から使えますが、推奨されていません。これは、これらのコンストラクタが型レベルでのみ使用可能であることを意味します。詳しくは[種システム](https://github.com/purescript/documentation/blob/master/language/Types.md#kind-system)を参照してください。

<!--
## `error` and `undefined`
-->
## `エラー`と`未定義`

<!--
For `error`, you can use `Effect.Exception.Unsafe.unsafeThrow`, in the `purescript-exceptions` package.
-->
`error`のために、`purescript-exceptions`パッケージの`Effect.Exception.Unsafe.unsafeThrow`を使えます。

<!--
`undefined` can be emulated with `Unsafe.Coerce.unsafeCoerce unit :: forall a. a`, which is in the `purescript-unsafe-coerce` package. See also https://github.com/purescript/purescript-prelude/issues/44.
-->
`undefined`は、`purescript-unsafe-coerce`パッケージの`Unsafe.Coerce.unsafeCoerce unit :: forall a. a`で模倣できます。https://github.com/purescript/purescript-prelude/issues/44 も見てください。

<!--
Although note that these might have different behaviour to the Haskell versions due to PureScript's strictness.
-->
しかし、PureScriptの正格性によってHaskellの場合とは異なる振る舞いをする可能性に注意してください。

<!--
## Documentation comments
-->
## 文書化コメント

<!--
When writing documentation, the pipe character `|` must appear at the start of every comment line, not just the first. See [the documentation for doc-comments](Syntax.md#comments) for more details.
-->
ドキュメントを書いている時には、全コメント行の先頭にパイプ文字`|`を書く必要があります。詳しくは[文書化コメントのための文書](Syntax.md#comments)を参照してください。

<!--
## Where is ... from Haskell?
-->
## Haskellの○○はどこにある？

<!--
As PureScript has not inherited Haskell's legacy code, some operators and functions that are common in Haskell have different names in PureScript:
-->
PureScriptはHaskellのレガシーコードを引き継いでいないので、いくつかの演算子や関数は異なる名前を持っています。

<!--
- `(>>)` is `(*>)`, as `Apply` is a superclass of `Monad` so there is no need to have an `Monad`-specialised version.
- Since 0.9.1, the `Prelude` library does not contain `(++)` as a second alias for `append` / `(<>)` (`mappend` in Haskell) anymore.
- `mapM` is `traverse`, as this is a more general form that applies to any traversable structure, not just lists. Also it only requires `Applicative` rather than `Monad`. Similarly, `liftM` is `map`.
- Many functions that are part of `Data.List` in Haskell are provided in a more generic form in `Data.Foldable` or `Data.Traversable`.
- `some` and `many` are defined with the type of list they operate on (`Data.Array` or `Data.List`).
- Instead of `_foo` for typed holes, use `?foo`. You have to name the hole; `?` is not allowed.
- Ranges are written as `1..2` rather than `[1..2]`. There's a difference, though: in Haskell `[2..1]` is an empty list, whereas in PureScript `2..1` is `[2,1]`
-->
- `(>>)`は`(*>)`です。`Apply`は`Monad`のスーパークラスなので、`Monad`に特化する必要がないからです。
- 0.9.1以来、`Prelude`ライブラリは`append`／`(<>)`(Haskellの`mappend`)の別名の別名である`(++)`を含んでいません。
- `mapM`は`traverse`です。これはリストだけでない任意の一筆書き可能な構造に適用されるより一般的な形で、`Monad`ではなく`Applicative`のみを必要とします。同様に、`liftM`は`map`です。
- Haskellの`Data.List`にある多くの関数は、より一般的な形で`Data.Foldable`または`Data.Traversable`から提供されます。
- `some`と`many`は、それらが操作する型のリストと共に`Data.Array`または`Data.List`で定義されています。
- 型付き穴には`_foo`の代わりに`?foo`を使います。穴には名前が必要で、`?`は許されません。
- レンジは`[1..2]`ではなく`1..2`と書かれます。Haskellの`[2..1]`は空リストですが、PureScriptの`2..1`は`[2,1]`になるという違いがあります。
