<!--
# Modules
-->
# モジュール

<!--
All code in PureScript is contained in a module. Modules are introduced using the `module` keyword:
-->
PureScriptの全てのコードはモジュールに含まれます。モジュールは`module`キーワードによって導入されます：

```purescript
module A where
  
id x = x
```

<!--
## Importing Modules
-->
## モジュールの読み込み

<!--
A module can be imported using the `import` keyword. This is called an "open import" - it will create aliases for all of the values and types in the imported module:
-->
モジュールは`import`キーワードを使うことで読み込まれます。これは「openインポート」と呼ばれ、モジュール内の全ての値や型の別名を作成します：

```purescript
module B where
  
import A
```

<!--
Alternatively, a list of names to import can be provided in parentheses:
-->
代わりに、カッコ内に指定して読み込むことも可能です：

```purescript
module B where
  
import A (runFoo)
```

<!--
Values, operators (wrapped in parentheses), type constructors, data constructors, and type classes can all be explicitly imported. A type constructor can be followed by a list of associated data constructors to import in parentheses. A double dot (`..`) can be used to import all data constructors for a given type constructor:
-->
値、演算子（丸括弧で囲んでおく）、型コンストラクタ、データコンストラクタ、型クラスは全て明示的に読み込むことができます。型コンスタクタの後の丸括弧内に関連するデータコンストラクタを書き連ねることもでき、ピリオドを2つ(`..`)書くと、型コンストラクタの全てのデータコンストラクタがインポートされます。

```purescript
module B where

import A (runFoo, (.~), Foo(..), Bar(Bar))
```

<!--
Type classes are imported using the `class` keyword, kinds with `kind`:
-->
型クラスをインポートするには`class`キーワードを使います。種の場合は`kind`です。

```purescript
module B where

import A (class Fab, kind Effect)
```

<!--
### Hiding imports
-->
### 除外インポート

<!--
It is also possible to exclude some names from an open import with the `hiding` keyword. This is useful to avoid import conflicts between modules:
-->
`hiding`キーワードを使って、openインポートから除外する名前を指定できます。これはモジュール間でインポートした名前が衝突することを防ぐときに便利です。

```purescript
module C where
  
import A hiding (runFoo)
import B (runFoo)
```

<!--
## Qualified Imports
-->
修飾インポート

<!--
Modules can also be imported qualified, which means that their names will not be brought directly into scope, but rather, aliased as a different module name.
-->
モジュールは修飾名付きでインポートすることもできます。つまり、モジュールの名前はそのままスコープに入れられるのではなく、別名として入れられます。

<!--
Following are some situations in which qualified imports are quite useful.
-->
以下は修飾インポートが非常に便利に使える場面です。

<!--
### Using generically-named functions
-->
### 総称関数の使用

``` purescript
import Data.Map as Map

a :: Map Int String
a = Map.fromFoldable [ Tuple 1 "a" ]
```

<!--
Several data structure modules have a `fromFoldable` function which can be used to create an instance of that data structure from any other `Foldable` data structure. To clarify which `fromFoldable` function is being used, we can import that module's functions under a qualified name and use it qualified, like `Set.fromFoldable`.
-->
いくつかのデータ構造モジュールは、別の`Foldable`データ構造からそのデータ構造のインスタンスを作成できる`fromFoldable`関数を持っています。どの`fromFoldable`関数が使われているのかを明確にするために、`Set.fromFoldable`のようにインポートの際に付けた修飾名のあとに関数名を書くことができます。

<!--
Another example, using a fictitious module this time:
-->
別の例として、ここでは架空のモジュールを使います：

``` purescript
import MyWebFramework as MyWebFramework

main :: Eff (dom :: DOM) Unit
main = do
  elem <- domElementById "appContainer"
  MyWebFramework.run elem
  -- ^ this may be more clear than
  -- `run elem`
```

<!--
Because "run" is a rather non-descript name, without knowing the type of a `run` function before reading it, it isn't clear what to expect the `run` function to do until we see that its module is `MyWebFramework`. To mitigate this confusion to new readers of this code, we can import and call it qualified - `MyWebFramework.run`.
-->
"run"はどこにでもありそうな名前なので、読む前にその関数の型を知らなければ、そのモジュールが`MyWebFramework`であると知るまでは`run`関数が何をするためのものなのかが不明確です。次の人がこのコードを読んだときの混乱を軽減するために、`MyWebFramework.run`のように修飾付きでインポートして使用することができます。

### Avoiding naming conflicts

```purescript
module Main where

import Data.Array as Array

null = ...

test = Array.null [1, 2, 3]
```

Here, the name ``null`` would ordinarily conflict with ``null`` from ``Data.Array``, but the qualified import solves this problem. ``Data.Array.null`` can be referenced using ``Array.null`` instead.

Operators can also be referenced this way:
```purescript
test' = Array.null ([1, 2, 3] Array.\\ [1, 2, 3])
```

### Merging modules

Modules can be merged under the same name using qualified imports. If merging multiple modules, consider using explicit imports to avoid conflicts, in case modules would want to import the same name:

```purescript
module Main where

import Data.String.Regex (split) as Re
import Data.String.Regex.Flags (global) as Re
import Data.String.Regex.Unsafe (unsafeRegex) as Re

split = Re.split (Re.unsafeRegex "[,;:.]\\s+" Re.global)
```

## Module Exports

You can control what gets exported from a module by using an export list. When an export list is used, other modules will only be able to see things which are in the export list. For example:

```purescript
module A (exported) where

exported :: Int -> Int
exported = [...]

notExported :: Int -> Int
notExported = [...]
```

In this case, modules importing `A` will not be able to see the `notExported` function.

The types of names which can be exported is the same as for module imports.

Imported modules can be re-exported in their entirety:

```purescript
module A (module B) where

import B
```

Qualified and explicit imports can be used also:

```purescript
module A (module MoreExports) where

import A.Util (useful, usefulFn) as MoreExports
import A.Type (ADatatype(..)) as MoreExports
```

When re-exporting other modules, all local values and types can also be exported by specifying the module itself as an export:

```purescript
module A (module A, module B) where

import B

data ...
```

### Exporting type classes

To export a type class, simply add it to the list, together with all of its members. Unfortunately there is no short-hand for exporting all type class members in one go.

For example, suppose we have the following:

```purescript
class Foldable f where
  foldr :: forall a b. (a -> b -> b) -> b -> (f a) -> b
  foldl :: forall a b. (b -> a -> b) -> b -> (f a) -> b
  foldMap :: forall a m. (Monoid m) => (a -> m) -> (f a) -> m
```

Then you can export the class with:

```purescript
module Test (class Foldable, foldr, foldl, foldMap) where
```

If a type class is exported, then all of its members must also be exported. Likewise, if a type class member is exported, the type class it belongs to must also be exported.
