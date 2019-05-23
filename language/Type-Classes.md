<!--
# Type Classes
-->
# 型クラス

<!--
PureScript supports type classes via the `class` and `instance` keywords.
-->
PureScriptは`class`と`instance`のキーワードによって型クラスをサポートしています。

<!--
Types appearing in class instances must be of the form `String`, `Number`, `Boolean`, or `C t1 ... tn` where `C` is a type constructor (including `->` and `t_i` are types of the same form).
-->
クラスインスタンスに現れる型は`String`、`Number`、`Boolean`または`C t1 ... tn`のような形でなければなりません。`C`は型コンストラクタです(`->`と`t_i`は
同じ形式の型)。

<!--
Here is an example of the `Show` typeclass, with instances for `String`, `Boolean` and `Array`:
-->
以下は`Show`型クラスのインスタンス`String`、`Boolean`、`Array`の例です：

```purescript
class Show a where
  show :: a -> String

instance showString :: Show String where
  show s = s

instance showBoolean :: Show Boolean where
  show true = "true"
  show false = "false"

instance showArray :: (Show a) => Show (Array a) where
  show xs = "[" <> joinWith ", " (map show xs) <> "]"
example = show [true, false]
```


<!--
Overlapping instances are no longer allowed in PureScript. To write overlapping instances, you should use Instance Chains.
-->
インスタンスの重複はPureScriptでは許されていません。インスタンスの重複を書くためには、インスタンスチェーンを使う必要があります。

<!--
## Instance Chains
-->
## インスタンスチェーン

<!--
PureScript implements a form of instance chains that work on groups of instances matching by parameters. This means that constraints are not considered when choosing instances. However, you can still write a chain of instances in consecutive order that will be matched top to bottom by using the `else` keyword.
-->
PureScriptでは、引数によってマッチするインスタンスのグループを処理するインスタンスチェーンを実装しています。これはインスタンスを選ぶときに制約が考慮されないことを意味します。しかし、`else`キーワードを使用して、上から下の順でマッチするインスタンスのチェーンを書くことが可能です。

<!--
Here is an example of a `MyShow` typeclass, with instances for `String`, `Boolean`, and any other type.
-->
以下は`MyShow`型クラスのインスタンス`String`、`Boolean`と任意の型の例です。

```purescript
class MyShow a where
  myShow :: a -> String

instance showString :: MyShow String where
  myShow s = s

else instance showBoolean :: MyShow Boolean where
  myShow true = "true"
  myShow false = "false"

else instance showA :: MyShow a where
  myShow _ = "Invalid"

data MysteryItem = MysteryItem

main = do
  log $ myShow "hello" -- hello
  log $ myShow true -- true
  log $ myShow MysteryItem -- Invalid
```

<!--
## Multi-Parameter Type Classes
-->
## 多変数型クラス

<!--
TODO. For now, see the section in [PureScript by Example](https://leanpub.com/purescript/read#leanpub-auto-multi-parameter-type-classes).
-->
TODO。 今は[PureScript by Example](https://leanpub.com/purescript/read#leanpub-auto-multi-parameter-type-classes)を参照してください。

<!--
## Superclasses
-->
## スーパークラス

<!--
Superclass implications can be indicated in a class declaration with a backwards fat arrow `<=`:
-->
スーパークラスの包含はクラス宣言の中で逆向きの太矢印`<=`を使って示します：

```purescript
class (Monad m) <= MonadFail m where
  fail :: forall a. String -> m a
```

<!--
This code example defines a `MonadFail` class with a `Monad` superclass: any type which defines an instance of `MonadFail` will be required to define an instance of `Monad` too.
-->
この例のコードは`Monad`をスーパークラスに持つ`MonadFail`クラスを宣言しています。`MonadFail`のインスタンスを定義する任意の型は`Monad`のインスタンスも定義していることを要求されます。

<!--
Superclass instances will be used when searching for an instance of a subclass. For example, in the code below, the `Applicative` constraint introduced by the `pure` function can be discharged since `Applicative` is a superclass of `Monad`, which is in turn a superclass of `MonadFail`:
-->
スーパークラスのインスタンスはサブクラスのインスタンスを探すときに使用されます。例えば、以下のコードでは、`Applicative`は`Monad`のスーパークラス、つまり`MonadFail`のスーパークラスであるため、`pure`関数によって導入された`Applicative`制約は解除できます。

```purescript
assert :: forall m. (MonadFail m) => Boolean -> m Unit
assert true = pure unit
assert false = fail "Assertion failed"
```

<!--
## Orphan Instances
-->
## 孤児インスタンス

<!--
Type class instances which are defined outside of both the module which defined the class and the module which defined the type are called *orphan instances*. Some programming languages (including Haskell) allow orphan instances with a warning, but in PureScript, they are forbidden. Any attempt to define an orphan instance in PureScript will mean that your program does not pass type checking.
-->
クラスを定義したモジュールと型を定義したモジュールの両方の外側で定義された型クラスのインスタンスを*孤児インスタンス*と呼びます。Haskellを含むいくつかのプログラミング言語は、孤児インスタンスを警告を伴って許可します。しかしPureScriptではこれを禁止しています。PureScriptで孤児インスタンスを定義しようとすると、あなたのプログラムは型検査を通りません。

<!--
For example, the `Semigroup` type class is defined in the module `Data.Semigroup`, and the `Int` type is defined in the module `Prim`. If we attempt to define a `Semigroup Int` instance like this:
-->
例えば、`Semigroup`型クラスはモジュール`Data.Semigroup`の中で定義されていて、`Int`型はモジュール`Prim`の中で定義されています。もし私達が次のように`Semigroup Int`インスタンスを定義しようとすると：

```purescript
module MyModule where

import Prelude

instance semigroupInt :: Semigroup Int where
  append = (+)
```

<!--
This will fail, because `semigroupInt` is an orphan instance. You can use a `newtype` to get around this:
-->
これは失敗します。`semigroupInt`は孤児インスタンスだからです。`newtype`を使うことでこれを回避できます：

```purescript
module MyModule where

import Prelude

newtype AddInt = AddInt Int

instance semigroupAddInt :: Semigroup AddInt where
  append (AddInt x) (AddInt y) = AddInt (x + y)
```

<!--
In fact, a type similar to this `AddInt` is provided in `Data.Monoid.Additive`, in the `monoid` package.
-->
実は、この`AddInt`に似た型が`monoid`パッケージの`Data.Monoid.Additive`で提供されています。

<!--
For multi-parameter type classes, the orphan instance check requires that the instance is either in the same module as the class, or the same module as at least one of the types occurring in the instance. (TODO: example)
-->
多変数型クラスのために、孤児インスタンス検査は、そのインスタンスがそのクラスと同じモジュール内にあるか、そのインスタンスに存在する型の少なくとも1つと同じモジュール内にあることを要求します。

<!--
## Functional Dependencies
-->
## 関数型依存性

<!--
Instances for type classes with multiple parameters generally only need a subset of the parameters to be concrete to match instances. Declarations on which parameters can determine others in instance heads are called Functional Dependencies. For example:
-->
一般的に、複数の引数を持つクラスのインスタンスは、マッチして具象になるために引数の一部のみを必要とします。どのパラメータがインスタンスヘッド内の他のパラメータを決定できるかという宣言を関数型依存性と呼びます。例えば：

```purescript
class TypeEquals a b | a -> b, b -> a where
  to :: a -> b
  from :: b -> a

instance refl :: TypeEquals a a where
  to a = a
  from a = a
```

<!--
The `|` symbol marks the beginning of functional dependencies, which are separated by a comma if there are more than one. In this case, the first parameter determines the type of the second, and the second determines the type of the first.
-->
`|`記号は関数型依存性の始まりを意味し、1つ以上ある場合はカンマで区切ります。この場合、1つ目の引数は2つ目の型を、2つ目の引数は1つ目の型を決定します。

<!--
Functional dependencies are especially useful with the various `Prim` typeclasses, such as `Prim.Row.Cons`: https://pursuit.purescript.org/builtins/docs/Prim.Row#t:Cons
-->
関数型依存性はさまざまな`Prim`型クラスで特に便利です。例えば、[`Prim.Row.Cons`](https://pursuit.purescript.org/builtins/docs/Prim.Row#t:Cons)などです。

<!--
See also the section in [PureScript by Example](https://leanpub.com/purescript/read#leanpub-auto-functional-dependencies).
-->
本の[該当する章(PureScript by Example)](https://leanpub.com/purescript/read#leanpub-auto-functional-dependencies)も見てください。

<!--
## Type Class Deriving
-->
## 型クラス導出

<!--
Some type class instances can be derived automatically by the PureScript compiler. To derive a type class instance, use the `derive instance` keywords:
-->
いくつかの型クラスインスタンスはPureScriptコンパイラによって自動的に導出されます。型クラスインスタンスを導出するためには、`derive instance`キーワードを使います：

```purescript
newtype Person = Person { name :: String, age :: Int }

derive instance eqPerson :: Eq Person
derive instance ordPerson :: Ord Person
```

<!--
Currently, the following type classes can be derived:
-->
現在、以下の型クラスが導出可能です：

<!--
- [Data.Generic.Rep (class Generic)](https://pursuit.purescript.org/packages/purescript-generics-rep/6.0.0/docs/Data.Generic.Rep#t:Generic)
- [Data.Eq (class Eq)](https://pursuit.purescript.org/packages/purescript-prelude/4.1.0/docs/Data.Eq#t:Eq)
- [Data.Ord (class Ord)](https://pursuit.purescript.org/packages/purescript-prelude/4.1.0/docs/Data.Ord#t:Ord)
- [Data.Functor (class Functor)](https://pursuit.purescript.org/packages/purescript-prelude/4.1.0/docs/Data.Functor#t:Functor)
- [Data.Newtype (class Newtype)](https://pursuit.purescript.org/packages/purescript-newtype/3.0.0/docs/Data.Newtype#t:Newtype)
-->
- [Data.Generic.Rep (Genericクラス)](https://pursuit.purescript.org/packages/purescript-generics-rep/6.0.0/docs/Data.Generic.Rep#t:Generic)
- [Data.Eq (Eqクラス)](https://pursuit.purescript.org/packages/purescript-prelude/4.1.0/docs/Data.Eq#t:Eq)
- [Data.Ord (Ordクラス)](https://pursuit.purescript.org/packages/purescript-prelude/4.1.0/docs/Data.Ord#t:Ord)
- [Data.Functor (Functorクラス)](https://pursuit.purescript.org/packages/purescript-prelude/4.1.0/docs/Data.Functor#t:Functor)
- [Data.Newtype (Newtypeクラス)](https://pursuit.purescript.org/packages/purescript-newtype/3.0.0/docs/Data.Newtype#t:Newtype)

<!--
## Compiler-Solvable Type Classes
-->
## コンパイラで解決可能な型クラス

<!--
Some type classes can be automatically solved by the PureScript Compiler without requiring you place a PureScript statement, like `derive instance`, in your source code.
-->
いくつかの型クラスは`derive instance`のようなキーワードを書くことなくPureScriptコンパイラによって自動的に解決可能です。

``` purescript
foo :: forall t. (Warn "Custom warning message") => t -> t
foo x = x
```

<!--
Automatically solved type classes are included in the [Prim](https://pursuit.purescript.org/builtins/docs/Prim) modules:
-->
自動的に解決される型は[Prim](https://pursuit.purescript.org/builtins/docs/Prim)モジュールに含まれています：

<!--
Symbol-related classes
-->
記号関連のクラス

- [`IsSymbol`](https://pursuit.purescript.org/packages/purescript-symbols/3.0.0/docs/Data.Symbol#t:IsSymbol)
- [`Append`](https://pursuit.purescript.org/builtins/docs/Prim.Symbol#t:Append)
- [`Compare`](https://pursuit.purescript.org/builtins/docs/Prim.Symbol#t:Compare)
- [`Cons`](https://pursuit.purescript.org/builtins/docs/Prim.Symbol#t:Cons)

[Prim.Row](https://pursuit.purescript.org/builtins/docs/Prim.Row)

- [`Cons`](https://pursuit.purescript.org/builtins/docs/Prim.Row#t:Cons)
- [`Union`](https://pursuit.purescript.org/builtins/docs/Prim.Row#t:Union)
- [`Nub`](https://pursuit.purescript.org/builtins/docs/Prim.Row#t:Nub)
- [`Lacks`](https://pursuit.purescript.org/builtins/docs/Prim.Row#t:Lacks)

[Prim.RowList](https://pursuit.purescript.org/builtins/docs/Prim.RowList)

- [`RowToList`](https://pursuit.purescript.org/builtins/docs/Prim.RowList#t:RowToList)

<!--
Other classes
-->
その他のクラス

- [`Partial`](https://pursuit.purescript.org/builtins/docs/Prim#t:Partial)
- [`Fail`](https://pursuit.purescript.org/builtins/docs/Prim.TypeError#t:Fail)
- [`Warn`](https://pursuit.purescript.org/builtins/docs/Prim.TypeError#t:Warn)
