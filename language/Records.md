<!--
# Records
-->
# レコード

<!--
Record literals are surrounded by braces, as in JavaScript:
-->
レコードリテラルはJavaScriptのように波括弧で囲みます：

```purescript
author :: { name :: String, interests :: Array String }
author =
    { name: "Phil"
    , interests: ["Functional Programming", "JavaScript"]
    }
```

<!--
Fields of records can be accessed using a dot, followed by the label of the field to access:
-->
レコードのフィールドには、ドットの後にフィールドのラベルを続けることでアクセスできます：

```purescript
> author.name
"Phil"

> author.interests
["Functional Programming","JavaScript"]
```

<!--
## Kinds
-->
## 種

<!--
`{ ... }` is just syntactic sugar for the `Record` type constructor, so `{ language ::  String }` is the same as `Record ( language :: String )`.
-->
`{ ... }`は単なる`Record`型コンストラクタの糖衣構文なので、`{ language ::  String }`は`Record ( language :: String )`と同じです。

<!--
The Record type constructor is parameterized by a row of types. In kind notation, `Record` has kind `# Type -> Type`. That is, it takes a row of types to a type.
-->
レコード型コンストラクタは型の列によってパラメータ化されています。型注釈では、`Record`は種`# Type -> Type`を持っています。これは型の列を取って型にするということです。

<!--
`( language :: String )` denotes a row of types (something of kind `# Type`), so it can be passed to `Record` to construct a type, namely `Record ( language :: String )`.
-->
`( language :: String )`は型の列(`# Type`など)を表すので、型を構築するために`Record`を通します。すなわち、`Record ( language :: String )`です。

## Extending Records

It is possible to define an extensible record

```purescript
type Lang l = { language :: String | l }
```

that can then be extended like:

```purescript
type Language = Lang ( country :: String )
```

The `Language` type synonym would then be equivalent to `{ language :: String, country :: String }`. Note that parentheses must be used for the extension, since `l` has to be a row kind not a record type.

## Wildcards

Record literals with wildcards can be used to create a function that produces the record instead:

```purescript
{ foo: _, bar: _ }
```
is equivalent to:

```purescript
\foo bar -> { foo: foo, bar: bar }
```

## Record Update

PureScript also provides a record update syntax similar to Haskell's:

```purescript
setX :: Number -> Point -> Point
setX val point = point { x = val }
```

This can be used to update nested records:

```purescript
setPersonPostcode :: PostCode -> Person -> Person
setPersonPostcode pc p = p { address { postCode = pc } }
```

## Field Names

Symbols which are illegal value identifiers, such as title-cased identifiers or ones containing spaces, can be used to identify a field by enclosing it in double-quotes:

```purescript
author' :: { "Name" :: String, "Personal Interests" :: Array String }
author' = { "Name": "Phil", "Personal Interests": ["Functional Programming", "JavaScript"] }

> author'."Name"
"Phil"

> (author' { "Name" = "John" })."Name"
"John"
```

If compiling to JavaScript, consider that the PureScript compiler will allow you to choose symbols which have special meaning in JavaScript, like `__proto__`.

```purescript
oops = {__proto__: unsafeCoerce ""}.__proto__.constructor.constructor "alert('lol')" 0
-- When loaded onto a web page, this will display "lol" in a browser dialog box,
--   which is an effectful behavior despite this expression appearing to be pure.
```
