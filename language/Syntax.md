<!--
# Syntax
-->
# 構文

<!--
## Whitespace Rules
-->
## 空白規則

<!--
Syntax is whitespace sensitive. The general rule of thumb is that declarations which span multiple lines should be indented past the column on which they were first defined on their subsequent lines.
-->
構文は空白に敏感です。一般的な経験則としては、複数行に分けられた宣言の2行目以降は1行目よりも深い位置にあるべきです。

<!--
That is, the following is valid:
-->
これは正しいです：

``` purescript
foo = bar +
  baz
```

<!--
But this is not:
-->
これは正しくありません：

``` purescript
foo = bar +
baz
```

<!--
## Comments
-->
## コメント

<!--
A single line comment starts with `--`:
-->
1行コメントは`--`で始まります：

``` purescript
-- This is a comment
```

<!--
Multi-line comments are enclosed in `{-` and `-}`:
-->
複数行コメントは`{-`と`-}`で囲みます：

``` purescript
{-
  Comment
  continued comment
-}
```

<!--
Comments that start with a pipe character, `|`, are considered documentation, and will appear in the output of tools like `psc-docs` and Pursuit. For example:
-->
パイプ文字（|）で始まるコメントはドキュメントと見なされ、ドキュメント生成ツール`psc-docs`の出力とPursuitで見えるようになります。例をお見せします：

``` purescript
-- | `bool` performs case analysis for the `Boolean` data type, like an `if` statement.
bool :: forall a. Boolean -> a -> a -> a
bool true x _ = x
bool false _ x = x
```

<!--
Note that, unlike Haskell, every line which should be considered documentation must start with a pipe. This allows you to do things like:
-->
Haskellとは異なり、ドキュメントに含めるべき行は全てパイプ文字から始まらなければなりません。したがって以下のようなことが可能です：


``` purescript
-- | Sort an array based on its `Ord` instance.
-- |
-- | This implementation runs in `O(n^2)` time, where `n` is the length of the
-- | input array.
-- TODO: try to optimise this?
sort :: forall a. (Ord a) => Array a -> Array a
sort xs = [...]
```

<!--
## Top-level declarations
-->
## トップレベル宣言

<!--
Values at the top level of a module are defined by providing a name followed by an equals sign and then the value to associate:
-->
モジュールのトップレベルの値は、名前の後に等号とそれに関連づける値を書くことで宣言されます：

``` purescript
one = 1
```

<!--
Functions can also be defined at the top level by providing a list of patterns on the left hand side of the equals sign:
-->
等号の左側にパターンの羅列を書くことで、トップレベルに関数を宣言することもできます。

``` purescript
add x y = x + y
```

<!--
See the section on pattern matching for more details about the kinds of patterns that can be used here.
-->
ここで使えるパターンの種類についてさらに詳しく知りたいなら、パターンマッチの章を見てください。

<!--
Functions using pattern matching may be defined multiple times to handle different pattern matches:
-->
パターンマッチを使う関数は異なるパターンに対応するために複数回定義されることがあります：

``` purescript
isEmpty [] = true
isEmpty _ = false
```

<!--
This does not mean functions can be arbitrarily overloaded with different numbers or types of arguments though.
-->
これは、関数が異なる数や引数の型によって勝手にオーバーロードされることを意味しているのではありません。。

<!--
Guards can also be used in these definitions:
-->
ガードはこのような定義にも使用できます：

``` purescript
isEmptyAlt xs | length xs == 0 = true
isEmptyAlt _ = false
```

<!--
A top level declaration is generally defined with a type signature:
-->
トップレベルの宣言は一般的に型シグネチャと共に定義されます。

```purescript
multiply :: Number -> Number -> Number
multiply x y = x * y
```

<!--
Type signatures are not required for top-level declarations in general, but is good practice to do so. See the section on types for more details.
-->
型シグネチャは一般的にはトップレベル宣言の際に必須ではありませんが、良い作法です。詳しくは型の章を見てください。

<!--
## Function application
-->
## 関数適用

<!--
Function application is indicated by just the juxtaposition of a function with its arguments:
-->
関数適用は関数とその引数を並べることで表します：

``` purescript
add 10 20
```

<!--
PureScript functions are defined as curried, so partial application has no special syntax:
-->
PureScriptの関数はカリー化されているので、部分適用に特別な構文は必要ありません：

``` purescript
add10 = add 10
```

<!--
In fact, `add 10 20` is parsed as `(add 10) 20`.
-->
実は、`add 10 20`は`(add 10) 20`として解析されます。

<!--
## Literals
-->
## リテラル

<!--
### Numbers
-->
### 数値

<!--
Numeric literals can be integers (type `Int`) or floating point numbers (type `Number`). Floating point numbers are identified by a decimal point.
Integers in hexadecimal notation should be preceded by the characters `0x`:
-->
数値リテラルは整数(`Int`型)または浮動小数点数(`Number`型)になります。浮動小数点数は小数点によって識別されます。
16進表記の整数の前には`0x`を書く必要があります：

``` purescript
16 :: Int
0xF0 :: Int
16.0 :: Number
```

<!--
### Strings
-->
### 文字列

<!--
String literals are enclosed in double-quotes and may extend over multiple lines. Line breaks should be surrounded by slashes as follows:
-->
文字列リテラルはダブルクォーテーションによって囲まれています。複数行にするためには、改行をバックスラッシュによって囲む必要があります。

``` purescript
"Hello World"

"Hello \
\World"
```

<!--
Line breaks will be omitted from the string when written this way.
-->
このように書かれているときには、改行は文字列から省略されます。

<!--
#### Triple-quote Strings
-->
#### トリプルクォート文字列

<!--
If line breaks are required in the output, they can be inserted with `\n`. Alternatively, you can use triple double-quotes to prevent special parsing of escaped symbols. This also allows the use of double quotes within the string with no need to escape them:
-->
出力に改行が必要なら文字列中に`\n`を含めることができますが、その代わりにダブルクォートを3つ使うことで、エスケープされた記号の特殊な解析を防ぐことができます：

``` purescript
jsIsHello :: String
jsIsHello = """
function isHello(greeting) {
  return greeting === "Hello";
}
"""
```

<!--
This method of declaring strings is especially useful when writing regular expression strings.
-->
この文字列宣言の方法は、正規表現文字列を書くときに特に便利です。

```
regex ".+@.+\\..+" noFlags
regex """.+@.+\..+""" noFlags
```

<!--
The example regular expression above is a very simple email address validator. Both are equivalent, but the second one, using triple double-quotes, is much easier to write and maintain. This is even more true when writing complex regular expressions, as many are.
-->
上記の正規表現は非常にシンプルなメールアドレス妥当性検証器です。2つの書き方は同等ですが、トリプルクォートを使う方が、書く時も保守する時も簡単です。これは複雑な正規表現を書くときに更に有用です。

<!--
### Booleans
-->
### 真理値

<!--
The boolean literals are `true` and `false`.
-->
真理値リテラルは`true`と`false`です。

<!--
### Functions
-->
### 関数

<!--
Function values (sometimes called _lambdas_) are introduced by using a backslash followed by a list of argument names:
-->
関数値（*ラムダ式*と呼ばれることもあります）はバックスラッシュのあとに引数リストを書くことで表現できます：

``` purescript
\a b -> a + b
```

<!--
which would correspond to the following JavaScript:
-->
これは以下のJavaScriptに対応します：

``` javascript
function (a) {
  return function (b) {
    return a + b;
  }
}
```

<!--
### Arrays
-->
### 配列

<!--
Array literals are surrounded by square brackets, as in JavaScript:
-->
配列リテラルはJavaScriptと同様に角括弧で囲まれています：

``` purescript
[]
[1, 2, 3]
```

<!--
### Records
-->
### レコード

<!--
Record literals are surrounded by braces, as in JavaScript:
-->
レコードリテラルはJavaScriptと同様に波括弧で囲まれています：

``` purescript
{}
{ foo: "Foo", bar: 1 }
```

<!--
Record literals with wildcards can be used to create a function that produces the record instead:
-->
ワイルドカードを含むレコードリテラル表記は、レコードを作成する関数を生成します：

``` purescript
{ foo: _, bar: _ }
```

これは以下と同等です：
is equivalent to:

``` purescript
\foo bar -> { foo: foo, bar: bar }
```

<!--
## Additional forms with Records
-->
## レコードの使い方

<!--
### Property Accessors
-->
### プロパティアクセサ

<!--
To access a property of a record, use a dot followed by the property name, as in JavaScript:
-->
レコードのプロパティにアクセスするためには、JavaScriptと同様に、ドットの後にプロパティ名を書きます：

``` purescript
rec.propertyName
```

<!--
There are also partially applied accessors, where an underscore is followed by a property name:
-->
部分適用のアクセサもあります。アンダースコアの後にプロパティ名を書きます：

``` purescript
_.propertyName
```

<!--
This is equivalent to:
-->
これは以下と同等です：

``` purescript
\rec -> rec.propertyName
```

<!--
These work with any number of levels:
-->
これはプロパティがいくつ並んでも可能です：

``` purescript
_.nested.property.name
```

<!--
### Record Updates
-->
### レコード更新

<!--
Properties on records can be updated using the following syntax:
-->
レコードのプロパティは以下の構文を使って更新できます：

``` purescript
rec { key1 = value1, ..., keyN = valueN, nestedKey { subKey = value, ... } }
```

<!--
Some or all of the keys may be updated at once, and records inside of records can also be updated.
-->
キーの全てまたは一部を一度に更新することもでき、レコード内のレコードも更新されます。

<!--
For example, the following function increments the `foo` property on its argument:
-->
例えば、以下の関数は引数のプロパティ`foo`の値をインクリメントします：

``` purescript
\rec -> rec { foo = rec.foo + 1 }
```

<!--
[Nested record updates](https://liamgoodacre.github.io/purescript/records/2017/01/29/nested-record-updates.html) look like this:
-->
[ネストされたレコードの更新](https://liamgoodacre.github.io/purescript/records/2017/01/29/nested-record-updates.html) look like this:

``` purescript
r = { val: -1
    , level1: { val: -1
              , level2: { val: -1 }
              }
    }
r' = r { level1 { val = 1 } }
```

<!--
Wildcards can also be used in updates to produce a partially applied update:
-->
ワイルドカードを更新に使用して、部分適用の更新を生成することも可能です：

``` purescript
rec { foo = _ }
```

<!--
This is equivalent to:
-->
これは以下と同等です：

``` purescript
\foo -> rec { foo = foo }
```

<!--
An underscore can also appear in the object position in an updater:
-->
アンダースコアは更新器のオブジェクトの位置に書くことも可能です：

``` purescript
_ { foo = 1 }
```

<!--
This is equivalent to:
-->
これは以下と同等です：

``` purescript
\rec -> rec { foo = 1 }
```

## Binary Operators

Operators in PureScript are just regular binary functions. In particular, no operators are built into the language; an overview of operators defined in libraries such as the `Prelude` is therefore outside the scope of this reference.

Operators can be defined by providing an operator alias for an existing function (which must be binary, i.e. its type must be of the form `a -> b -> c`). For example:

``` purescript
data List a = Nil | Cons a (List a)

append :: forall a. List a -> List a -> List a
append xs Nil = xs
append Nil ys = ys
append (Cons x xs) ys = Cons x (append xs ys)

infixr 5 append as <>
```

This function can be used as follows::

```purescript
oneToThree = Cons 1 (Cons 2 (Cons 3 Nil))
fourToSix = Cons 4 (Cons 5 (Cons 6 Nil))

oneToSix = oneToThree <> fourToSix
```

Operator alias declarations are made up of four parts:

* The associativity: either `infixl`, `infixr`, or `infix`.
* The precedence: an integer, between 0 and 9. Here, it is 5.
* The function to alias: here, `append`
* The operator: here, `<>`.

The declaration determines how expressions involving this operator are bracketed.

### Associativity

`infixl` means that repeated applications are bracketed starting from the left. For example, `#` from Prelude is left-associative, meaning that an expression such as:

```
products # filter isInStock # groupBy productCategory # length
```

is bracketed as:

```
((products # filter isInStock) # groupBy productCategory) # length
```

Similarly, `infixr` means "right-associative", and repeated applications are bracketed starting from the right. For example, `$` from Prelude is right-associative, so an expression like this:

```
length $ groupBy productCategory $ filter isInStock $ products
```

is bracketed as:

```
length $ (groupBy productCategory $ (filter isInStock $ products))
```

`infix` means "non-associative". Repeated use of a non-associative operator is disallowed. For example, `==` from Prelude is non-associative, which means that

```
true == true == true
```

results in a [NonAssociativeError](../errors/NonAssociativeError.md):

```
Cannot parse an expression that uses multiple instances of the non-associative operator Data.Eq.(==).
Use parentheses to resolve this ambiguity.
```

Non-associative parsing via `infix` is most appropriate for operators `f` for which ``x `f` (y `f` z)`` is not necessarily the same as ``(x `f` y) `f` z)``, that is, operators which do not satisfy the algebraic property of associativity.

### Precedence

Precedence determines the order in which operators are bracketed. Operators with a higher precedence will be bracketed earlier. For example, take `*` and `+` from Prelude. `*` is precedence 7, whereas `+` is precedence 6. Therefore, if we write:

```
2 * 3 + 4
```

then this is bracketed as follows:

```
(2 * 3) + 4
```

Operators of different associativities may appear together as long as they do not have the same precedence. This restriction exists because the compiler is not able to make a sensible choice as to how to bracket such expressions. For example, the operators `==` and `<$>` from the Prelude have the fixities `infix 4` and `infixl 4` respectively, which means that given the expression

```
f <$> x == f <$> y
```

the compiler does not know whether to bracket it as

```
(f <$> x) == (f <$> y)
```

or

```
f <$> (x == f) <$> y
```

Therefore, we get a [MixedAssociativityError](../errors/MixedAssociativityError):

```
Cannot parse an expression that uses operators of the same precedence but mixed associativity:

  Data.Functor.(<$>) is infixl
  Data.Eq.(==) is infix

Use parentheses to resolve this ambiguity.
```

### Operators as values

Operators can be used as normal values by surrounding them with parentheses:

``` purescript
and = (&&)
```

### Operator sections

Operators can be partially applied by surrounding them with parentheses and using `_` as one of the operands:

``` purescript
half = (_ / 2)
double = (2 * _)
```

### Functions as operators

Functions can be used as infix operators when they are surrounded by backticks:

``` purescript
foo x y = x * y + y
test = 10 `foo` 20
```

Operator sections also work for functions used this way:

``` purescript
fooBy2 = (_ `foo` 2)
```

## Case expressions

The `case` and `of` keywords are used to deconstruct values to create logic based on the value's constructors. You can match on multiple values by delimiting them with `,` in the head and cases.

``` purescript
f :: Maybe Boolean -> Either Boolean Boolean -> String
f a b = case a, b of
  Just true, Right true -> "Both true"
  Just true, Left _ -> "Just is true"
  Nothing, Right true -> "Right is true"
  _, _ -> "Both are false"
f (Just true) (Right true)
```

Like top-level declarations, `case` expressions support guards.
``` purescript
f :: Either Int Unit -> String
f x = case x of
  Left x | x == 0 -> "Left zero"
         | x < 0 -> "Left negative"
         | otherwise -> "Left positive"
  Right _ -> "Right"
```

A binding can be avoided by using a single underscore in place of the expression to match on; in this context the underscore represents an _anonymous argument_.
``` purescript
case _ of
  0 -> "None"
  1 -> "One"
  _ -> "Some"
```

This is equivalent to
```purescript
\x -> case x of
  0 -> "None"
  1 -> "One"
  _ -> "Some"
```



## If-Then-Else expressions

The `if`, `then` and `else` keywords can be used to create conditional expressions similar to a JavaScript ternary expression. The `else` block is always required:

``` purescript
conditional = if 2 > 1 then "ok" else "oops"
```

## Let and where bindings

The `let` keyword introduces a collection of local declarations, which may be mutually recursive, and which may include type declarations:

``` purescript
factorial :: Int -> Int
factorial =
  let
    go :: Int -> Int -> Int
    go acc 1 = acc
    go acc n = go (acc * n) (n - 1)
  in
    go 1
```

The `where` keyword can also be used to introduce local declarations at the end of a value declaration:

``` purescript
factorial :: Int -> Int
factorial = go 1
  where
  go :: Int -> Int -> Int
  go acc 1 = acc
  go acc n = go (acc * n) (n - 1)
```

## Indentation in binding blocks

Indentation of a binding's body is significant. If defining multiple bindings, as in a let-in block, each binding must have the same level of indentation. The body of the binding's definition, then, must be further indented. To illustrate:

``` purescript
f =
  -- The `let-in` block starts at an indentation of 2 spaces, so
  --   the bindings in it must start at an indentation greater than 2.
  let
    -- Because `x` is indented 4 spaces, `y` must also be indented 4 spaces.
    -- Its body, then, must have indentation greater than 4 spaces.
    x :: Int -> Int
    x a =
      -- This body is indented 2 spaces.
      a
    y :: Int -> Int
    y c =
        -- This body is indented 4 spaces.
        c
  in do
    -- Because `m` is indented 4 spaces from the start of the `let`,
    --   `n` must also be indented 4 spaces.
    -- Its body, then, must be greater than 4 spaces.
    let m =
          -- This body is indented 2 spaces.
          x (y 1)
        n =
            -- This body is indented 4 spaces.
            x 1
    log "test"
```

## Do notation

The `do` keyword introduces simple syntactic sugar for monadic expressions.

Here is an example, using the monad for the [`Maybe`](https://github.com/purescript/purescript-maybe) type:

``` purescript
maybeSum :: Maybe Number -> Maybe Number -> Maybe Number
maybeSum a b = do
  n <- a
  m <- b
  let result = n + m
  pure result
```

`maybeSum` takes two values of type ``Maybe Number`` and returns their sum if neither value is `Nothing`.

When using `do` notation, there must be a corresponding instance of the `Monad` type class for the return type.

Statements can have the following form:

- `a <- x` which desugars to `x >>= \a -> ...`
- `x` which desugars to `x >>= \_ -> ...` or just `x` if this is the last statement.
- A let binding `let a = x`. Note the lack of the `in` keyword.

The example `maybeSum` desugars to::

``` purescript
maybeSum a b =
  a >>= \n ->
    b >>= \m ->
      let result = n + m
      in pure result
```
Note: (>>=) is the `bind` function for the `Bind` type as defined in the [Prelude package](https://pursuit.purescript.org/packages/purescript-prelude/4.1.0/docs/Prelude#t:Bind).
