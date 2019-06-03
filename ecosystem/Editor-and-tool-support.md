<!--
## Editors
-->
## エディタ

#### Atom 

<!--
- [purescript-contrib/atom-language-purescript](https://github.com/purescript-contrib/atom-language-purescript) provides syntax highlighting
- [nwolverson/atom-ide-purescript](https://github.com/nwolverson/atom-ide-purescript) provides build support,   REPL, and autocomplete etc. via [psc-ide](https://github.com/purescript/purescript/tree/master/psc-ide)
-->
- [purescript-contrib/atom-language-purescript](https://github.com/purescript-contrib/atom-language-purescript) 構文ハイライトを提供します
- [nwolverson/atom-ide-purescript](https://github.com/nwolverson/atom-ide-purescript) ビルド支援、REPL、自動補完を[psc-ide](https://github.com/purescript/purescript/tree/master/psc-ide)を通じて提供します

#### Emacs

<!--
 Either use these two:
- [dysinger/purescript-mode](https://github.com/dysinger/purescript-mode) was adapted from haskell-mode
- [epost/psc-ide-emacs](https://github.com/epost/psc-ide-emacs) offers Emacs support for [psc-ide](https://github.com/purescript/purescript/tree/master/psc-ide)
-->
 次の2つを使います：
- [dysinger/purescript-mode](https://github.com/dysinger/purescript-mode) haskell-modeから影響を受けています
- [epost/psc-ide-emacs](https://github.com/epost/psc-ide-emacs) [psc-ide](https://github.com/purescript/purescript/tree/master/psc-ide)のEmacsサポートを提供します

<!--
Or these two, for a more minimal setup:
-->
もしくは、更に小さい設定のためには次の2つを使います：

<!--
- [emacs-pe/purescript-mode](https://github.com/emacs-pe/purescript-mode) is an alpha-stage greenfield mode
- [emacs-pe/flycheck-purescript](https://github.com/emacs-pe/flycheck-purescript) provides flycheck support.
-->
- [emacs-pe/purescript-mode](https://github.com/emacs-pe/purescript-mode) alpha-stageのグリーンフィールドモードです
- [emacs-pe/flycheck-purescript](https://github.com/emacs-pe/flycheck-purescript) flycheckをサポートしています

<!--
PSCI support via comint:
-->
comintを通じてPSCIをサポートするもの：

<!--
- [ardumont/psci-mode](https://github.com/ardumont/emacs-psci) is a REPL minor mode
-->
- [ardumont/psci-mode](https://github.com/ardumont/emacs-psci) REPLのマイナーモードです

<!--
Spacemacs users can just use the [`purescript` layer](https://github.com/syl20bnr/spacemacs/tree/master/layers/%2Blang/purescript).
-->
Spacemacsユーザは[`purescript`層](https://github.com/syl20bnr/spacemacs/tree/master/layers/%2Blang/purescript)を使えます。

#### Sublime Text 2

<!--
- [PureScript package](https://sublime.wbond.net/search/PureScript) by joneshf
-->
- [PureScriptパッケージ](https://sublime.wbond.net/search/PureScript) 作者：joneshf

#### Vim

<!--
- [purescript-vim](https://github.com/raichoo/purescript-vim) syntax highlighting and indentation
-->
- [purescript-vim](https://github.com/raichoo/purescript-vim) 構文ハイライトと字下げ
- [FrigoEU/psc-ide-vim](https://github.com/FrigoEU/psc-ide-vim/)

#### VS Code

- [nwolverson/vscode-ide-purescript](https://github.com/nwolverson/vscode-ide-purescript)

<!--
#### General
-->
#### 全般

<!--
- To generate `TAGS` files, use `purs docs --format etags` (or `--format ctags`)
-->
- `purs docs --format etags`(または`--format ctags`)を使用して`TAGS`ファイルを生成できます。

<!--
## Build tools and package managers
-->
## ビルドツールとパッケージマネージャ

<!--
- [Pulp](https://github.com/purescript-contrib/pulp) - a standalone build system for PureScript ([pulp](https://www.npmjs.com/package/pulp) in `npm`)
- [psc-package](https://github.com/purescript/psc-package):  A package manager for PureScript based on the concept of package sets
- [Gulp task](https://github.com/purescript-contrib/gulp-purescript) (`gulp-purescript` in `npm`)
- [psvm-js](https://github.com/ThomasCrvsr/psvm-js) - PureScript Version Manager
- [purs-loader](https://github.com/ethul/purs-loader/) - PureScript loader for WebPack
- [psc-pane](https://github.com/anttih/psc-pane) - Auto reloading compiler which formats a single error to fit the window
- [purescript-psa](https://github.com/natefaubion/purescript-psa) - A pretty, flexible error/warning reporting frontend for `psc` featuring colours, original source spans in errors, warning filtering and persistence.
- [pscid](https://github.com/kRITZCREEK/pscid) is a lightweight file-watcher/testrunner for PS projects, that uses `psc-ide` to provide fast rebuilds.
-->
- [Pulp](https://github.com/purescript-contrib/pulp) - PureScript (`npm`の[pulp](https://www.npmjs.com/package/pulp))用のスタンドアロン型ビルドシステム
- [psc-package](https://github.com/purescript/psc-package): - パッケージ集合の概念を基にしたPureScriptパッケージマネージャ
- [Gulp task](https://github.com/purescript-contrib/gulp-purescript) (`npm`の`gulp-purescript`)
- [psvm-js](https://github.com/ThomasCrvsr/psvm-js) - PureScriptバージョンマネージャ
- [purs-loader](https://github.com/ethul/purs-loader/) - WebPack用のPureScriptローダ
- [psc-pane](https://github.com/anttih/psc-pane) - ウインドウに合うように1つのエラーを整形する自動リロードコンパイラ
- [purescript-psa](https://github.com/natefaubion/purescript-psa) - 色付き、元のエラー範囲、警告のフィルタリング、永続性を特徴とする、`psc`のための可愛く柔軟な、エラー／警告を報告するフロントエンド
- [pscid](https://github.com/kRITZCREEK/pscid) `psc-ide`を使用して高速再ビルドを行う、PureScriptプロジェクト用の軽量ファイルウォッチャー／テストランナー
