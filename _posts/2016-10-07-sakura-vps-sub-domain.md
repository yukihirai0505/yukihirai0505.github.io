---
layout: post
title:  "さくらのVPSでサブドメインの設定"
date:   2016-10-07 13:09:43 +0900
categories: Development
---

今回は今までドメイン直下にフォルダを作成して
サーバー上で動かしてきたプロジェクトをサブドメインにしたいなあと思い設定してみました。

もともと `http://yukihirai0505.com/portfolio/` のような形で動かしていたサービスを

[http://portfolio.yukihirai0505.com/](http://portfolio.yukihirai0505.com/)

といった形で設定してみました。

## サブドメインとは

サブドメインは上記の例で言うと、

http://portfolio.yukihirai0505.com の

`portfolio` の部分のことを言います。

ここの設定はさくらのVPSの場合だと下記のサイトがわかりやすく説明してくれていました。
→[さくらVPSのサブドメイン設定方法](http://mekori.hatenablog.com/entry/2013/04/20/231157)

このように設定していけば、一個ドメイン取得してしまえば自由にサブドメインを設定していくことができます。

## nginxでバーチャルホストの設定

上記のサイトの方法でサブドメインを登録すればあとはWEBサーバー側でリバースプロキシの設定をしてあげればいいだけです。
自分はnginxを使用しているので、
今回作成したファイルを見てみると

[https://gist.github.com/yukihirai0505/09bedf8ea5e4dcd3d2d3](https://gist.github.com/yukihirai0505/09bedf8ea5e4dcd3d2d3)

このような感じになります。
一回覚えてしまえばかなり手軽に設定できるので、
ドメインをお持ちの方はぜひお試しを^^
