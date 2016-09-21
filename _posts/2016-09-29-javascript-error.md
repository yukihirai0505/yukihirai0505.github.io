---
layout: post
title:  "JavascriptでUncaught SyntaxError: Unexpected token ILLEGALのエラー、Chromeのディベロッパーツールでファイルを見てみると謎の・・・がファイルの末尾に。。。"
date:   2016-09-29 13:09:43 +0900
categories: Development
---

開発していて久々にハマったことがあったのでブログに記載しておきます。

あるJavascriptファイルに変更を加えたいと思い、
少し編集すると謎のエラーがでました。

なんやこら。。。

ちなみにこれは入力した文字数分だけでてきます。

謎のエラー。

```
Uncaught SyntaxError: Unexpected token ILLEGAL
```

でも、他の開発者では再現しない。

これはもしや自分の開発環境が原因か、と思いググってみるとこんな記事が。

[http://qiita.com/ariarijp/items/ab1b4cee634893b89a37](http://qiita.com/ariarijp/items/ab1b4cee634893b89a37)

今回、プログラミング言語はPHPを使用しての開発だったので、
他の方々はMAMPを入れてましたが、
自分はVirtualBoxで環境を作っていました。

そのせいでこのようなエラーが起きてしまっていたようです。

Nginxの設定をApacheに置き換えて、
httpd.confのファイルを書き直しました。

```
EnableSendfile off
```

[http://qiita.com/KoriCori/items/29c827b4f9879206cc75](http://qiita.com/KoriCori/items/29c827b4f9879206cc75)

その後Apacheを再起動したら無事復活。

めちゃめちゃ気持ち悪いエラーだったw

謎の・でググったりしても何も出ず苦しんだw
他の人の環境で再現しないときは自分の環境を疑うようにしよう。
