![PureScript](https://github.com/purescript/purescript/raw/master/logo.png)

<!--
Welcome to the PureScript documentation repository!
-->
PureScriptのドキュメントリポジトリへようこそ！

<!--
PureScript is a small, strongly typed programming language that compiles to JavaScript.
To get a better overview of PureScript, visit [The PureScript Website](http://purescript.org).
-->
PureScriptは、小さくて、JavaScriptへコンパイルされる強い型付け言語です。
PureScriptの概要をもっと見たいなら、[The PureScript Website](http://purescript.org)にアクセスしてください。

<!--
This repository is a collaborative effort, so please feel free to make a pull request to add/edit content or create an issue to discuss it. PureScript is a big project used by people coming from a variety of backgrounds. Making documentation useful to a wide variety of people is really hard to do well, requiring readers like you to point out and add documentation you feel is missing. Thanks for helping!
-->
このリポジトリはみんなの努力の結晶ですから、編集や追加、プルリクエストをしたり、議論を重ねることを厭わず、気軽に行ってください。PureScriptは様々なバックグラウンドを持つ人々に利用される巨大なプロジェクトです。多様な人々にとって便利なドキュメントを作ることは本当に難しいため、内容の不足を指摘してくれるあなたのような読者を必要としています。よろしくお願いします！

<!--
## Directory
-->
## リンク集

<!--
### Getting Started
-->
### 入門

<!--
- [Getting Started](guides/Getting-Started.md): Download PureScript and build your first project
- [PureScript By Example](https://leanpub.com/purescript/read): A book about PureScript. Learn functional programming for the web by solving practical problems
- [Try PureScript](http://try.purescript.org): Try PureScript in your browser
-->
- [Getting Started](guides/Getting-Started.md): PureScriptのダウンロードから初めてのプロジェクトをビルドする方法までを解説しています。
- [PureScript By Example](https://leanpub.com/purescript/read): PureScriptについての本です。実用的な問題解決法をWebに役立てる関数型プログラミングの手法を学びます。
- [Try PureScript](http://try.purescript.org): PureScriptをWebブラウザから試すことができます。

<!--
### Learning
-->
### 学習

<!--
- The [PureScript Book](https://leanpub.com/purescript/read) is the recommended approach to learning the language, since it covers more material in greater depth. However, it covers `0.11.7` and is not updated yet for the `0.12.x` version of the compiler. Thus, one should be aware of the following materials when reading through the book:
    - See [dwhitney's fork of the book's exercises](https://github.com/dwhitney/purescript-book) which is updated for `0.12.x`.
    - See [Justin's `0.11.7` to `0.12.x` summary](https://purescript-resources.readthedocs.io/en/latest/0.11.7-to-0.12.0.html) to know how to 'translate' the outdated book's code into working code.
    - Be wary of any references to these [deprecated packages](https://github.com/purescript-deprecated) in the book.
- [Language Reference](language/README.md)
- [PureScript: Jordan's Reference](https://github.com/JordanMartinez/purescript-jordans-reference): An up-to-date project covering Getting Started, Build Tools, PureScript's syntax with examples, FP design patterns, and PureScript's ecosystem.
- [A guide to the PureScript numeric hierarchy](https://a-guide-to-the-purescript-numeric-hierarchy.readthedocs.io/en/latest/index.html): An introduction to the mathematics behind the numeric hierarchy of type classes in PureScript’s Prelude. (See also [PureScript numeric hierarchy overview](https://harry.garrood.me/numeric-hierarchy-overview/).)
-->
- [PureScript Book](https://leanpub.com/purescript/read) は、PureScriptに関するより多くのことを深く学習するためにおすすめの手段ですが、対応するコンパイラのバージョンが`0.11.7`のままで、`0.12.x`には未対応です。そのため、以下の記事の内容も合わせて知っておくべきです。
    - [dwhitney's fork of the book's exercises](https://github.com/dwhitney/purescript-book) `0.12.x`に対応しています。
    - [Justin's `0.11.7` to `0.12.x` summary](https://purescript-resources.readthedocs.io/en/latest/0.11.7-to-0.12.0.html) 古くなった本に含まれるソースコードを正しく動くように「翻訳」する方法を知ることができます。
    - [deprecated packages](https://github.com/purescript-deprecated) ここに含まれる内容には常に注意を払ってください（廃止予定のモジュール集）。
- [Language Reference](language/README.md)
- [PureScript: Jordan's Reference](https://github.com/JordanMartinez/purescript-jordans-reference): 入門、ビルドツール、PureScriptの構文の例、関数型プログラミングのデザインパターン、PureScriptのエコシステムなどをカバーする最新のプロジェクト集です。
- [A guide to the PureScript numeric hierarchy](https://a-guide-to-the-purescript-numeric-hierarchy.readthedocs.io/en/latest/index.html): PureScriptのPreludeに含まれる型クラスの数値の階層に隠れた数学についての解説です。[PureScript numeric hierarchy overview](https://harry.garrood.me/numeric-hierarchy-overview/)も参照してください。

<!--
### Guides
-->
### ガイド集

<!--
- [Common Operators](guides/Common-Operators.md)
- [The Foreign Function Interface (FFI)](guides/FFI.md)
- [FFI Tips](guides/FFI-Tips.md)
- [Custom Type Errors](guides/Custom-Type-Errors.md)
- [PureScript Without Node](guides/PureScript-Without-Node.md)
- [Contrib Library Guidelines](guides/Contrib-Guidelines.md)
- [Error Suggestions](guides/Error-Suggestions.md)
- [psc-ide FAQ](guides/psc-ide-FAQ.md)
- [Try PureScript Help](https://github.com/purescript/trypurescript/blob/gh-pages/README.md)
-->
- [演算子](guides/Common-Operators.md)
- [外部関数インタフェース(FFI)](guides/FFI.md)
- [外部関数インタフェース Tips集](guides/FFI-Tips.md)
- [Custom Type Errors](guides/Custom-Type-Errors.md)
- [Nodeを使わないPureScript](guides/PureScript-Without-Node.md)
- [準標準ライブラリ(Contrib Library)のガイドライン](guides/Contrib-Guidelines.md)
- [エラー・警告を指摘するツール](guides/Error-Suggestions.md)
- [psc-ideについてのよくある質問集](guides/psc-ide-FAQ.md)
- [Try PureScriptについて](https://github.com/purescript/trypurescript/blob/gh-pages/README.md)

<!--
### Tools
-->
### ツール

<!--
- [Editor and tool support](ecosystem/Editor-and-tool-support.md): Editor plugins, build tools, and other development tools
- [PureScript and NixOS](https://pr06lefs.wordpress.com/2015/01/11/get-started-with-purescript-on-nixos/): How to use PureScript with NixOS
- [PSCi](guides/PSCi.md): An interactive development tool for PureScript
-->
- [エディタとツール](ecosystem/Editor-and-tool-support.md): エディタプラグインとビルドツール、その他開発ツールについてのリンク集です。
- [PureScript and NixOS](https://pr06lefs.wordpress.com/2015/01/11/get-started-with-purescript-on-nixos/): NixOS上でPureScriptを使う方法の解説です。
- [PSCi](guides/PSCi.md): PureScriptの対話式開発ツールについての解説です。

<!--
### Ecosystem
-->
### エコシステム

<!--
- [Maintained Packages](ecosystem/Maintained-Packages.md)
- [Style Guide](guides/Style-Guide.md)
- [Alternate Backends](https://github.com/purescript/documentation/blob/master/ecosystem/Alternate-backends.md): PureScript can compile to other languages as well!
-->
- [メンテナンスされたパッケージ](ecosystem/Maintained-Packages.md)
- [スタイルガイド](guides/Style-Guide.md)
- [別のコンパイルターゲット](https://github.com/purescript/documentation/blob/master/ecosystem/Alternate-backends.md): PureScriptを別の言語にコンパイルすることが可能です!

<!--
### Articles
-->
### 記事集

- [24 Days of PureScript 2016](https://github.com/paf31/24-days-of-purescript-2016)

<!--
### Talks/Meetups
-->
### トーク・会合

- [PureScript Presentations](ecosystem/PureScript-Presentations.md)
- [PureScript Meetups](ecosystem/PureScript-Meetups.md)

<!--
### Related Languages
-->
### 関連のある言語

<!--
- [Related Projects](Related-Projects.md)
- [Differences from Haskell](language/Differences-from-Haskell.md)
-->
- [関連するプロジェクト](Related-Projects.md)
- [Haskellとの違い](language/Differences-from-Haskell.md)


<!--
## Project Scope
-->
## プロジェクトの範囲

<!--
Topics currently in this repository's scope:
-->
現在、このプロジェクトは以下のトピックを扱っています：

<!--
- PureScript language reference documentation
- Its compiler errors
- Core concepts on which the language is based
- Comparison with similar languages
- An introduction to other sources of documentation
-->
- PureScriptの言語リファレンス・ドキュメント
- コンパイラのエラー
- PureScriptの基盤となるコアコンセプト
- 似た言語との比較
- 別の提供元にあるドキュメントの紹介

<!--
Topics currently *not* in scope:
-->
現在、このプロジェクトは以下のトピックを**扱っていません**:

<!--
- Using PureScript libraries (those docs belong with the corresponding libraries)
- A PureScript language teaching course (use the [PureScript by Example](https://leanpub.com/purescript/read) book or other resources)
- Introduction to package managers and dependency management
-->
- PureScriptライブラリの使用 (これらは各ライブラリに付属しています)
- PureScriptの学習コース ([PureScript by Example](https://leanpub.com/purescript/read)やその他の記事を参照してください)
- パッケージマネージャと依存関係の管理についての紹介

<!--
Feel free to make an issue to discuss amending the scope.
-->
どうぞ気軽に、issueを立てたり、プロジェクト範囲の見直しについて話し合ってください。
