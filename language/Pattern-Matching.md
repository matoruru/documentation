<!--
# Pattern Matching
-->
# パターンマッチ

<!--
Pattern matching deconstructs a value to bring zero or more expressions into scope. Pattern matches are introduced with the `case` keyword.
-->
パターンマッチは値を分解して0個以上の式をスコープに入れます。パターンマッチは`case`キーワードにより導入されます。

<!--
Pattern matches have the following general form:
-->
パターンマッチは一般的に次の形式で表されます：

```purescript
case value of
  pattern -> result
  ...
  pattern -> result
```

<!--
Pattern matching can also be used in the declaration of functions, as we have already seen:
-->
既に見たように、パターンマッチは関数の宣言にも用いられます：

```purescript
fn pattern_1 ... pattern_n = result
```

<!--
Patterns can also be used when introducing functions. For example:
-->
パターンは関数を導入する際にも用いられます。例えば：

```purescript
example x y z = x * y + z
```

<!--
The following forms can be used for matching:
-->
以下のようなパターンが存在します：

<!--
- Wildcard patterns
- Literal patterns
- Variable patterns
- Array patterns
- Constructor patterns
- Record patterns
- Named patterns
-->
- ワイルドカードパターン
- リテラルパターン
- 変数パターン
- 配列パターン
- コンストラクタパターン
- レコードパターン
- 名前付きパターン

<!--
Guards and pattern guards are also supported.
-->
ガードとパターンガードもサポートされています。

<!--
The exhaustivity checker will introduce a `Partial` constraint for any pattern which is not exhaustive.
By default, patterns must be exhaustive, since this `Partial` constraint will not be satisfied. The error can be silenced, however, by adding a local `Partial` constraint to your function.
-->
網羅性検査器は網羅されていないパターンのために`Partial`制約を導入します。
デフォルトでは、`Partial`制約が満たされないために、パターンが網羅されていなければなりません。あなたの関数にローカルな`Partial`制約を追加することで、エラーを非表示にすることが可能です。

<!--
Wildcard Patterns
-----------------
-->
ワイルドカードパターン
----------------------

<!--
The wildcard `_` matches any input and brings nothing into scope:
-->
ワイルドカード`_`はどんな入力にもマッチし、スコープには何も入れません：

```purescript
f _ = 0
```

<!--
Literal Patterns
----------------
-->
リテラルパターン
----------------

<!--
Literal patterns are provided to match on primitives:
-->
リテラルパターンは定数とマッチさせるために用いられます：

```purescript
f true = 0
f false = 1

g "Foo" = 0
g _ = 1

h 0 = 0
h _ = 1
```

<!--
Variable Patterns
-----------------
-->
変数パターン
------------

<!--
A variable pattern matches any input and binds that input to its name:
-->
変数パターンはどんな入力にもマッチし、それを変数の名前に束縛します：

```purescript
double x = x * 2
```

<!--
Array Patterns
--------------
-->
配列パターン
------------

<!--
Array patterns match an input which is an array, and bring its elements into scope. For example:
-->
配列パターンは配列の入力にマッチし、その要素をスコープに入れます。例えば：

```purescript
f [x] = x
f [x, y] = x * y
f _ = 0
```

<!--
Here, the first pattern only matches arrays of length one, and brings the first element of the array into scope.
-->
ここで、1つ目のパターンは長さ1の配列にのみマッチし、その最初の要素をスコープに入れます。

<!--
The second pattern matches arrays with two elements, and brings the first and second elements into scope.
-->
2つ目のパターンは2つの要素を持つ配列にマッチし、その1番目と2番めの要素をスコープに入れます。

<!--
Constructor patterns
--------------------
-->
コンストラクタパターン
----------------------

<!--
Constructor patterns match a data constructor and its arguments:
-->
コンストラクタパターンはデータコンストラクタとその引数にマッチします：

```purescript
data Foo = Foo String | Bar Number Boolean

foo (Foo s) = true
foo (Bar _ b) = b
```

<!--
Record Patterns
---------------
-->
レコードパターン
----------------

<!--
Record patterns match an input which is a record, and bring its properties into scope:
-->
レコードパターンはレコードの入力にマッチし、そのプロパティをスコープに入れます：

```purescript
f { foo: "Foo", bar: n } = n
f _ = 0
```

<!--
Nested Patterns
---------------
-->
ネストしたパターン
------------------

<!--
The patterns above can be combined to create larger patterns. For example:
-->
このパターンは既に紹介したパターンを組み合わせて表現されます。例えば：

```purescript
f { arr: [x, _], take: "firstOfTwo" } = x
f { arr: [_, x, _], take: "secondOfThree" } = x
f _ = 0
```

<!--
Named Patterns
--------------
-->
名前付きパターン
----------------

<!--
Named patterns bring additional names into scope when using nested patterns. Any pattern can be named by using the ``@`` symbol:
-->
名前付きパターンはネストしたパターンを使っているときに、新たな名前をスコープに入れます。``@``記号を用いてどんなパターンも名付けられます：

```purescript
f a@[_, _] = a
f _ = []
```

<!--
Here, in the first pattern, any array with exactly two elements will be matched and bound to the variable `a`.
-->
ここで、1つ目のパターンでは、正確に2つの要素を持つ配列にマッチし、配列は変数`a`に束縛されます。

<!--
Guards
------
-->
ガード
------

<!--
Guards are used to impose additional constraints inside a pattern using boolean-valued expressions, and are introduced with a pipe after the pattern:
-->
ガードは条件式を用いて追加の制約を課すために用いられ、パターンの後にパイプを書くことで導入されます：

```purescript
evens :: List Int -> Int
evens Nil = 0
evens (Cons x xs) | x `mod` 2 == 0 = 1 + evens xs
evens (Cons _ xs) = evens xs
```

<!--
When using patterns to define a function at the top level, guards appear after all patterns:
-->
トップレベルで関数を定義するためにパターンを用いるとき、ガードは全パターンの後に現れます:

```purescript
greater x y | x > y = true
greater _ _ = false
```

<!--
To be considered exhaustive, guards must clearly include a case that is always true. Even though the following makes perfect sense, the compiler cannot determine that is is exhaustive:
-->
網羅的に考えるために、ガードは常に明らかに真になる条件を含まなければなりません。以下を完璧に作っても、コンパイラはそれが網羅的であることを検出できません：

```purescript
compare :: Int -> Int -> Ordering
compare x y
    | x > y  = GT
    | x == y = EQ
    | x < y  = LT
```

<!--
Either of these will work, since they clearly include a final case:
-->
以下はどちらも動き、最後のケースを含むことで網羅性が明確になっています：

```purescript
compare x y
    | x > y = GT
    | x < y = LT
    | otherwise = EQ

compare x y | x > y = GT
compare x y | x < y = LT
compare _ _ = EQ
```

<!--
(The name `otherwise` is a synonym for `true` commonly used in guards.)
-->
(`otherwise`は`true`の別名で、一般的にはガード内で使われます。)

<!--
Pattern Guards
--------------
-->
パターンガード
--------------

<!--
Pattern guards extend guards with pattern matching, notated with a left arrow. A pattern guard will succeed only if the computation on the right side of the arrow matches the pattern on the left.
-->
パターンガードは左矢印で表記されるパターンマッチでガードを拡張します。パターンガードは、矢印の右側の計算結果が左側のパターンとマッチする場合にのみ成功します。

<!--
For example, we can apply a function `fn` to an argument `x`, succeeding only if
`fn` returns `Just y` for some `y`, binding `y` at the same time:
-->
例えば、関数`fn`を引数`x`に適用することができます。`fn`が`Just y`を返すときにのみ成功します。`y`は同時に束縛されます：

```purescript
bar x | Just y <- fn x = ... -- x and y are both in scope here
```

<!--
Pattern guards can be very useful for expressing certain types of control flow when
using algebraic data types.
-->
パターンガードは、代数的データ型を使用する時、制御フローの特定の型を表現するために特に有用です。

<!--
You can also use commas to add multiple expressions in a guard:
-->
ガードに複数の式を追加するためにカンマを使うこともできます：

```purescript
positiveLessThanFive :: Maybe Int -> Boolean
positiveLessThanFive mInt
  | Just x <- mInt
  , x > 0
  , x < 5 = true
  | otherwise = false
```
