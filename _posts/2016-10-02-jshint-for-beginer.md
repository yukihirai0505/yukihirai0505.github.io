---
layout: post
title:  "初心者はJSHint(ESLint/GJSLint)を使って綺麗なJavascriptを書けるようにしよう。"
date:   2016-10-02 13:09:43 +0900
categories: Development
---

僕みたいなJS初心者は、
ゴリゴリJSを書いていると動くところまではいったものの
コードを見返してみると斬新(?)な書き方になっていることが多いです。(僕だけでしょうか。)

そこで今回、変な書き方してると指摘してくれるツールを教えて頂いたのでそちらを導入してみました。

その名もJSHintです。
今回は調子に乗ってESLintとGJSLintも使っているのでそちらも合わせて見ていきます。

## JSHintとは

まずJSHintについて見ていきます。
JSHintとは、
> JSHintとは、Javascript用構文チェッカーです。
構文チェッカーとしては、かなり厳しめのチェックをしてくれるJSLintがありますが、
これをforkして融通効くようにしたものがJSHintです。
[Developers.IO](http://dev.classmethod.jp/tool/jshint-javascript-sublime-text2/)より引用)

つまり、JSHintを使用すれば、
自分の書いてるJavascriptをチェックしてくれるのです。

インストールするのにはNode.jsが必要なので、忘れずにインストールしておきましょう。

インストールできたらコマンドラインから

```
npm install -g jshint
```

これでjshintをインストールできます。

インストールできたら適当にJSHintの設定ファイル(.jshintrc)をホームディレクトリで作成しましょう。

[https://gist.github.com/yukihirai0505/a2c3c589365dc8b09828](https://gist.github.com/yukihirai0505/a2c3c589365dc8b09828)

ひとまずこのくらいでいいかと思います。
他にもいろいろ見てみたい方はこちらのサイトから確認できます。
[http://jshint.com/docs/](http://jshint.com/docs/)

ひとまずこれでJSHintをインストールできたので

コマンドラインから

```
jshint [jsファイル]
```

みたいな感じで叩いてあげれば

> jquery.js: line 232, col 0, Identifier 'core_slice' is not in camel case.
jquery.js: line 238, col 18, Expected '===' and instead saw '=='.
jquery.js: line 277, col 0, Identifier 'core_slice' is not in camel case.

こんな感じでチェックしていけてないところを教えてくれます。

## JSHintをVimとかSublimeText2で使用する場合

いちいちコマンドラインでjshintを叩くのは面倒臭いので、
VimとSublimeText2で設定してみました。

##Vimの場合

まずVimの場合ですが、

プラグインを入れていきます。
プラグインの管理にはNeoBundleが便利なので、こちらを使用しています。

NeoBundleのインストールはこんな感じでどうぞ

[https://gist.github.com/yukihirai0505/d00f9cd3c9622f26ab5d](https://gist.github.com/yukihirai0505/d00f9cd3c9622f26ab5d)

次にホームディレクトリで.vimrcファイルを設定しましょう。

[https://gist.github.com/yukihirai0505/20fdf1edb42f44a90037](https://gist.github.com/yukihirai0505/20fdf1edb42f44a90037)

ここまでできたらまずはシンタックスチェック系のこちらのプラグインをインストールします。

```
NeoBundle 'scrooloose/syntastic'
```

これを設定ファイルに追加していきます。

[https://gist.github.com/yukihirai0505/e76108e927581c1d1b93](https://gist.github.com/yukihirai0505/e76108e927581c1d1b93)

これでVimを開いたらscrooloose/syntasticをインストールするかどうか聞かれるのでインストールしておきましょう。

次に.vimrcファイルに設定を追加していきます。

[https://gist.github.com/yukihirai0505/a8a8fcbfbc560d1db1ec](https://gist.github.com/yukihirai0505/a8a8fcbfbc560d1db1ec)

ひとまずこんな感じで設定してあげればVimで編集するときにJSHintの構文チェックが動いてくれます。
この場合はファイル開くときとかセーブするときとか構文チェックが走ってくれます。
ちょっとうざいかもしれませんのでそこらへんはお好みで設定をどうぞ！

## SublimeText2でJSHintを使用したい場合

次にSublimeText2でJSHintを使用する場合について見ていきます。
まずは[Package Control](https://packagecontrol.io/installation)をインストールしてください。

インストールできたら、

`コマンド+Shift+pで"Package Control: Install Package"`
を選択します。
そこでSublimeLinerをインストールしましょう。

これでインストールが完了すれば、
次回からセーブ時にJSHintが動いてくれます。

## 調子に乗ってESLintとかGJSLintも入れてみた

今回は、ESLintとGJSLintもインストールして使ってみてます。
ESLintについてはここを見るとわかりやすいです。

[ESLintについてのメモ](http://qiita.com/makotot/items/822f592ff8470408be18)

上記の記事にもありますが、

ESLintはJSHint同等の機能を持つ他、
解析ルールが完全にプラガブルになっているので、
独自ルールを自由に追加できるという特徴があります。

JSHintと同じく

```
npm install -g eslint
```

でインストールします。

次にGSLintですが、
こちらは
Google JavaScript Style Guideに則ってチェック・修正を行えるツールです。

インストールは

[https://gist.github.com/yukihirai0505/8d1e623bfa62edeecb9c](https://gist.github.com/yukihirai0505/8d1e623bfa62edeecb9c)

これでオッケーです。

あとはVimに設定を追加してあげれば使用できます。

[https://gist.github.com/yukihirai0505/b99e30b2ccb83ab40ee0](https://gist.github.com/yukihirai0505/b99e30b2ccb83ab40ee0)

これでオッケーです。

ちなみにVimでファイルを編集しているときに、

`:SyntasticInfo`

としてあげれば

```
Syntastic version: 3.6.0-156 (Vim 703, Darwin)
Info for filetype: javascript
Global mode: active
Filetype javascript is active
The current file will be checked automatically
Available checkers: eslint gjslint jshint
Currently enabled checkers: eslint jshint gjslint
```

こんな
感じで今動いているものなどを確認できます。

是非、構文チェックツールで脱"斬新な書き方"を目指していきたいものです。
