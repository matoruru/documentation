<!--
The Foreign Function Interface allows you to interact with code written in JavaScript (or whichever backend you're using).
-->
外部関数インタフェース(FFI)は、JavaScriptまたはバックエンドで使っている他の言語で書かれたコードとの共存を実現します。

<!--
Importing Values
----------------
-->
値のインポート
------------

<!--
The `foreign import` keywords declare a value which is defined in JavaScript, and its type:
-->
`foreign import`キーワードはJavaScriptで定義された値や型を宣言します：

```purescript
-- src/Math.purs
module Math where
foreign import pow :: Number -> Number -> Number
```

<!--
When importing values from the FFI, the values themselves are defined in separate files. The filename should be the same as the PureScript source file name, except with the ".purs" replaced with a file extension depending on the backend. For example, if the above code had been written in `src/Math.purs`, then the corresponding FFI file would be at `src/Math.js`, and it might look like this:
-->
値をFFIからインポートするとき、値は分離されたファイルに定義されています。そのファイル名とPureScriptのソースファイル名は、拡張子「.purs」がバックエンドの言語の拡張子に置き換わっている部分を除いて同じであるべきです。例えば、上のコードは`src/Math.purs`に書かれており、対応するFFIファイルは`src/Math.js`です。以下のようになるかもしれません：

```javascript
// src/Math.js
"use strict";
exports.pow = function(x) {
  return function(y) {
    return Math.pow(x,y);
  };
};
```

<!--
The compiler will check that your FFI file does export any values that you have imported via `foreign import`. However, the compiler cannot check that values defined in the FFI have the correct runtime representation based on the type they are given: it is your responsibility to ensure that, if you have declared that a value you imported is an `Int`, this value is actually an `Int` (and not an array, or function, or anything else).
-->
コンパイラは、PureScriptファイルが`foreign import`を通じてインポートした値がFFIファイルで公開されていることを確かめます。しかし、コンパイラはFFIファイルで定義された値の実行時表現に正しい型を与えているかどうかまでは確かめられないので、その責務はあなた自身にあります。インポートした値を`Int`として宣言したなら、その値が真に`Int`であるようにしてください（配列や関数などではなく）。

<!--
Importing Types
---------------
-->
型のインポート
------------

<!--
To declare a new abstract type (with no constructors), use `foreign import data` and provide the kind:
-->
新しい抽象型(コンストラクタがないもの)を宣言するためには、`foreign import data`を使って種を与えます：

```purescript
foreign import data DOMElement :: Type

foreign import document :: {
  createElement :: String -> DOMElement
}
```

<!--
When declaring types in this way, you may declare your type to have any kind, not just `Type`. For example, to declare a row of effects:
-->
この方法で型を宣言しているときは、単に`Type`ではなく、何らかの種を持つように型を宣言するかもしれません。例えば、副作用の種を宣言するには次のようにします：

```purescript
foreign import data MyRow :: # Effect
```
