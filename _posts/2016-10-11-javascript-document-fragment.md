---
layout: post
title:  "DocumentFragmentを使用してJSの処理を高速化"
date:   2016-10-11 13:09:43 +0900
categories: Development
---

今回も同じ会社のエンジニアに教えてもらったことを
備忘録も兼ねて記事に残していきます。

## DocumentFragmentとは？

DocumentFragmentはノードの一種です。
ノードなのでappendChildなどで子ノードを追加したりすることができます。
仮に下記のようなコードの場合にDocumentFragmentを使用しない場合を見ていきます。

{% gist yukihirai0505/8dd13d540d38febae494 %}

こちらはjQueryを使用してAPIからJSONでオブジェクトを取得し、
成功したときに取得したオブジェクトの値をテーブルにappendしていくといった内容です。
このときに

`$tbody.append($tr);`

ここの部分がオブジェクトの数だけ実行されます。
appendするということは画面を書き直しているので、
いちいちブラウザにアクセスしているような感じになります。
それでは数が多くなればなるほど処理が重たくなっていってしまいます。

こんなときにDocumentFragmentが役に立ちます。
DocumentFragmentを使用すると下記のような感じになります。

{% gist yukihirai0505/7e14f1fe2c5d4a04f89f %}

DocumentFragmentは先述した通りノードの一種なので、
appendしていくことができます。
ここでは最後に

```
$(<span class="pl-s"><span class="pl-pds">'</span>#users-table tbody<span class="pl-pds">'</span></span>).append($fragment);
```

このようにしているので、
ブラウザを書き直すのは一回になります。
どれだけオブジェクトの中身がたくさん返ってきても、
一回です。

このように最後に `$fragment` を追加しても、

DocumentFragment自体はappendされません。
つまり、tbodyの直下にはDocumentFragmentではなく、

`var $tr` のところで作成している `<tr>` タグが配置されます。

大量の要素をDOMに追加する際には活用していきたいです。

明日は今日書いたクソコードをリファクタリングせねばです。
