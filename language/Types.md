<!--
# Types
-->
# 型

<!--
The type system defines the following types:
-->
型システムは以下の型を定義します：

<!--
- Primitive Types: `Int`, `Number`, `String`, `Char`, `Boolean`
- Arrays
- Records
- Tagged Unions
- Newtypes
- Functions
- Polymorphic Types
- Constrained Types
- Type Synonyms
- Rows
-->
- 原始型: `Int`, `Number`, `String`, `Char`, `Boolean`
- 配列
- レコード
- タグ付き共用体
- ユーザ定義型
- 関数
- 多相型
- 制約のある型
- 型同義語(型シノニム)
- 列

<!--
## Primitive Types
-->
## 原始型

<!--
The primitive types `String`, `Number` and `Boolean` correspond to their JavaScript equivalents at runtime.
-->
`String`、`Number`、`Boolean`の各原始型は、JavaScriptのそれと対応しています。

<!--
## Integers
-->
## 整数型

<!--
The `Int` type represents integer values. The runtime representation is also a normal JavaScript number; however, operations like `(+)` on `Int` values are defined differently in order to ensure that you always get `Int` values as a result.
-->
`Int`型は整数値を表現します。ランタイムではJavaScriptの普通の数字と同じですが、`Int`値を受け取る演算子`(+)`などは、常に`Int`値を返すように、異なる定義になっています。

<!--
## Arrays
-->
## 配列

<!--
PureScript arrays correspond to Javascript arrays at runtime, but all elements in an array must have the same type. The `Array` type takes one type argument to specify what type this is. For example, an array of integers would have the type `Array Int`, and an array of strings would have the type `Array String`.
-->
PureScriptの配列はランタイムでJavaScriptの配列と対応していますが、全要素が同じ型でなければなりません。`Array`型は、引数を1つ取ることで、自分自身の型が何であるかを特定します。例えば、整数の配列は型`Array Int`を持ち、文字列の配列は型`Array String`を持ちます。

<!--
## Records
-->
## レコード

<!--
PureScript records correspond to JavaScript objects. They may have zero or more named fields, each with their own types. For example: `{ name :: String, greet :: String -> String }` corresponds to a JavaScript object with precisely two fields: `name`, which is a `String`, and `greet`, which is a function that takes a `String` and returns a `String`.
-->
PureScriptのレコードはJavaScriptのオブジェクトに対応しています。これは0個以上の名前付きフィールドを持ち、各フィールドが型を持っています。例えば：`{ name :: String, greet :: String -> String }`はJavaScriptオブジェクトの、`String`型の`name`と、`String`型を取り`String`型を返す関数`greet`の2フィールドに正確に対応します。

<!--
## Tagged Unions
-->
## タグ付き共用体

<!--
Tagged unions consist of one or more constructors, each of which takes zero or more arguments.
-->
タグ付き共用体は1つ以上のコンストラクタから構成されます。各コンストラクタは0個以上の引数を取ります。

<!--
Tagged unions can only be created using their constructors, and deconstructed through pattern matching (a more thorough treatment of pattern matching will be provided later).
-->
タグ付き共用体はコンストラクタからのみ作成され、パターンマッチによって解体されます（さらに徹底的なパターンマッチの方法については後に提示します）。

<!--
For example:
-->
例をお見せします：

```purescript
data Foo = Foo | Bar String

runFoo :: Foo -> String
runFoo Foo = "It's a Foo"
runFoo (Bar s) = "It's a Bar. The string is " <> s

main = do
  log (runFoo Foo)
  log (runFoo (Bar "Test"))
```

<!--
In the example, Foo is a tagged union type which has two constructors. Its first constructor `Foo` takes no arguments, and its second `Bar` takes one, which must be a String.
-->
上記の例で、Fooは2つのコンストラクタを持つタグ付き共用体です。1つ目のコンストラクタ`Foo`は引数を取らず、2つ目のコンストラクタ`Bar`はString型の引数を1つ取ります。

<!--
`runFoo` is an example of pattern matching on a tagged union type to discover its constructor, and the last two lines show how to construct values of type `Foo`.
-->
`runFoo`はタグ付き共用体のコンストラクタを発見する例になっています。また、最後の2行で`Foo`型の値を作成しています。

<!--
## Newtypes
-->
## ユーザ定義型

<!--
Newtypes are like data types (which are introduced with the `data` keyword), but are restricted to a single constructor which contains a single argument. Newtypes are introduced with the `newtype` keyword:
-->
ユーザ定義型は`data`キーワードによって表現されるデータ型と似ていますが、1つの引数を取るコンストラクタを1つ持つように制限されています。ユーザ定義型は`newtype`キーワードによって表現されます。

```purescript
newtype Percentage = Percentage Number
```

<!--
The representation of a newtype at runtime is the same as the underlying data type. For example, a value of type `Percentage` is just a JavaScript number at runtime.
-->
ユーザ定義型は、ランタイムではその基となったデータ型と同じです。例えば、`Percentage`型の値は、ランタイムではただのJavaScriptの数値です。

<!--
Newtypes are considered different from their underlying types by the type checker. For example, if you try to apply a function to a `Percentage` where it expects a `Number`, the type checker will reject your program.
-->
ユーザ定義型は、型検査によって、その基となった型とは異なるものとして扱われます。例えば、`Number`型を受け取る関数に`Percentage`型を渡すことは、型検査によって拒絶されます。

<!--
Newtypes can be assigned their own type class instances, so for example, `Percentage` can be given its own `Show` instance:
-->
ユーザ定義型には、自身にクラスのインスタンスを割り当てることができます。例えば、`Percentage`は`Show`インスタンスを持つことができます：

```purescript
instance showPercentage :: Show Percentage where
  show (Percentage n) = show n <> "%"
```

<!--
## Functions
-->
## 関数

<!--
Functions in PureScript are like their JavaScript counterparts, but always have exactly one argument.
-->
PureScriptの関数はJavaScriptのそれとほぼ対応しますが、確実に1つの引数を持つようになっています。

<!--
## Polymorphic Types
-->
## 多相型

<!--
Expressions can have polymorphic types:
-->
式は多相型を持つことができます：

```purescript
identity x = x
```

<!--
``identity`` is inferred to have (polymorphic) type ``forall t0. t0 -> t0``. This means that for any type ``t0``, ``identity`` can be given a value of type ``t0`` and will give back a value of the same type.
-->
``identity``は多相型の``forall t0. t0 -> t0``を持つ、と推論されます。これは、``identity``が何の型にでもなれる``t0``型の値を受け取り、同じ型の値を返すことを意味します。

<!--
A type annotation can also be provided:
-->
以下のように型注釈をつけることもできます：

```purescript
identity :: forall a. a -> a
identity x = x
```

<!--
## Row Polymorphism
-->
## 列多相

<!--
Polymorphism is not limited to abstracting over types. Values may also be polymorphic in types with other kinds, such as rows or effects (see "[Kind System](#kind-system)").
-->
多相性は型の抽象化に制限がなく、値は種に関して多相になるかもしれません。例えば、列やモナドなどです（[Kind System](#kind-system)を参照してください。）。


<!--
For example, the following function accesses two properties on a record:
-->
例えば、以下の関数はレコードの2パラメータにアクセスします。

```purescript
addProps o = o.foo + o.bar + 1
```

<!--
The inferred type of ``addProps`` is:
-->
``addProps``から推論された型は：

```purescript
forall r. { foo :: Int, bar :: Int | r } -> Int
```

<!--
Here, the type variable ``r`` has kind ``# Type`` - it represents a row of types. It can be instantiated with any row of named types.
-->
ここで、型変数``r``は種``# Type``を持ちます。この種は型の列を表現しています。これはどんな型の指定でもインスタンス化することができます。

<!--
In other words, ``addProps`` accepts any record which has properties ``foo`` and ``bar``, and *any other record properties*.
-->
別の言い方をすると、``addProps``は``foo``と``bar``という2つのプロパティに加え、どんなプロパティを持つレコードでもいいということです。

<!--
Therefore, the following application compiles:
-->
したがって、以下のアプリケーションがコンパイルできます：

```purescript
addProps { foo: 1, bar: 2, baz: 3 }
```

<!--
even though the type of ``addProps`` does not mention the property ``baz``. However, the following does not compile:
-->
``addProps``はプロパティ``baz``について言及していないにも関わらず、です。しかし、以下のようにするとコンパイルできません：

```purescript
addProps { foo: 1 }
```

<!--
since the ``bar`` property is missing.
-->
プロパティ``bar``が欠落しているからです。

<!--
## Rank N Types
-->
## 高階多相型

<!--
It is also possible for the ``forall`` quantifier to appear on the left of a function arrow, inside types record fields and data constructors, and in type synonyms.
-->
``forall``量化子を関数アローの左側や、レコード型のフィールド、データコンストラクタ、型同義語に書くことも可能です。

<!--
In most cases, a type annotation is necessary when using this feature.
-->
ほとんどの場合、この機能を使うときには型注釈が必要になります。

<!--
As an example, we can pass a polymorphic function as an argument to another function:
-->
私達が多相関数を他の関数の引数として渡すことができる例をお見せします：

```purescript
poly :: (forall a. a -> a) -> Boolean
poly f = (f 0 < 1) == f true
```

<!--
Notice that the polymorphic function's type argument is instantiated to both ``Number`` and ``Boolean``.
-->
多相関数の型引数が``Number``と``Boolean``の両方にインスタンス化されることに注意してください。

<!--
An argument to ``poly`` must indeed be polymorphic. For example, the following fails:
-->
``poly``の引数は多相でなければなりません。例えば、以下のようにすると失敗します：

```purescript
test = poly (\n -> n + 1)
```

<!--
since the skolemized type variable ``a`` does not unify with ``Int``.
-->
スコーレム化された変数``a``は必ずしも``Int``になるわけではないからです。

<!--
## Rows
-->
## 列

<!--
A row of types represents an unordered collection of named types, with duplicates. Duplicate labels have their types collected together in order, as if in a ``NonEmptyList``. This means that, conceptually, a row can be thought of as a type-level ``Map Label (NonEmptyList Type)``.
-->
型の列は、名前付きでの型から成る重複可能で順不同な集合です。重複するラベルは順番に型がまとめられ、``NonEmptyList``のようになります。これは概念的には、列は型レベルの``Map Label (NonEmptyList Type)``であると考えられるということです。

<!--
Rows are not of kind ``Type``: they have kind ``# k`` for some kind ``k``, and so rows cannot exist as a value. Rather, rows can be used in type signatures to define record types or other type where labelled, unordered types are useful.
-->
列は``Type``種ではなく、``k``種に対しての``# k``種を持つので、値として存在することはできません。むしろ、列はレコード型やラベル付けされた別の型を定義するために型シグネチャとして使用されます。非順序型は便利です。

<!--
To denote a closed row, separate the fields with commas, with each label separated from its type with a double colon:
-->
閉じた列を表現するためには、フィールドをカンマで区切ります。また、フィールドと型はコロン2つで区切ります：

```purescript
( name :: String, age :: Number )
```

<!--
To denote an open row (i.e. one which may unify with another row to add new fields), separate the specified terms from a row variable by a pipe:
-->
開いた列（新しいフィールドを追加するために他の行と統合する可能性がある）を表現するためには、特定の文字と行変数をパイプで区切ります：

```purescript
( name :: String, age :: Number | r )
```

## Type Synonyms

For convenience, it is possible to declare a synonym for a type using the ``type`` keyword. Type synonyms can include type arguments but [cannot be partially applied](../errors/PartiallyAppliedSynonym.md#partiallyappliedsynonym-error). Type synonyms can be built with any other types but [cannot refer to each other in a cycle](../errors/CycleInTypeSynonym.md#cycleintypesynonym-error).

For example:

```purescript
-- Create an alias for a record with two fields
type Foo = { foo :: Number, bar :: Number }

-- Add the two fields together, since they are Numbers
addFoo :: Foo -> Number
addFoo o = o.foo + o.bar

-- Create an alias for a polymorphic record with the same shape
type Bar a = { foo :: a, bar :: a }
-- Foo is now equivalent to Bar Number

-- Apply the fields of any Bar to a function
combineBar :: forall a b. (a -> a -> b) -> Bar a -> b
combineBar f o = f o.foo o.bar

-- Create an alias for a complex function type
type Baz = Number -> Number -> Bar Number

-- This function will take two arguments and return a record with double the value
mkDoubledFoo :: Baz
mkDoubledFoo foo bar = { foo: 2.0*foo, bar: 2.0*bar }

-- Build on our previous functions to double the values inside any Foo
-- (Remember that Bar Number is the same as Foo)
doubleFoo :: Foo -> Foo
doubleFoo = combineBar mkDoubledFoo

-- Define type synonyms to help write complex Effect rows
-- This will accept further Effects to be added to the row
type RandomConsoleEffects eff = ( random :: RANDOM, console :: CONSOLE | eff )
-- This limits the Effects to just RANDOM and CONSOLE
type RandomConsoleEffect = RandomConsoleEffects ()
```

Unlike newtypes, type synonyms are merely aliases and cannot be distinguished from usages of their expansion. Because of this they cannot be used to declare a type class instance. For more see [``TypeSynonymInstance`` Error](../errors/TypeSynonymInstance.md#typesynonyminstance-error).

## Constrained Types

Polymorphic types may be predicated on one or more ``constraints``. See the chapter on type classes for more information.

## Type Annotations

Most types can be inferred (not including Rank N Types and constrained types), but annotations can optionally be provided using a double-colon, either as a declaration or after an expression:

```purescript
-- Defined in Data.Semiring
one :: forall a. (Semiring a) => a

-- one can be an Int, since Int is an instance of Semiring where one = 1
int1 :: Int
int1 = one -- same as int1 = 1
-- Or even a Number, which also provides a Semiring instance where one = 1.0
number1 = one :: Number -- same as number1 = 1.0
-- Or its polymorphism can be kept, so it will work with any Semiring
-- (This is the default if no annotation is given)
semiring1 :: forall a. Semiring a => a
semiring1 = one
-- It can even be constrained by another type class
equal1 = one :: forall a. Semiring a => Eq a => a
```

## Kind System

The kind system defines the following kinds:

- ``Type``, the kind of types.
- Arrow kinds ``k1 -> k2``
- Row kinds ``# k``
- User-defined kinds, such as ``Control.Monad.Eff.Effect``, the kind of effects.

## Row Kinds

The kind ``# k`` of rows is used to classify labelled, unordered collections of types of kind ``k``.

For example ``# Type`` is the kind of rows of types, as used to define records, and ``# Control.Monad.Eff.Effect`` is the kind of rows of effects, used to define the monad ``Control.Monad.Eff.Eff`` of extensible effects.

## Quantification

A type variable can refer to not only a type or a row, but a type constructor, or row constructor etc., and type variables with those kinds can be bound inside a ``forall`` quantifier.
