---
layout: post
title:  "CSSを教わりつつMaterial Design Liteを使ってみた。"
date:   2016-10-08 13:09:43 +0900
categories: Development
---

外でばんばん花火の音がなっております。
部屋の中で聞く花火の音はどこか切なさを感じさせてくれます。笑
熱海の花火大会に行きたい。

また今日は久々に高校時代の友人に会いましたが、
久しぶりに会って昔話に花を咲かせました。
いま思えばかなり特殊な環境で過ごした高校3年間だったなあと思います。笑

さて今週は、ひたすらコードを書いた週になりました。
多分、仕事を始めてから一番書きました。笑

職場の方にCSSの基礎を改めて教わり、
Material Design Liteも使ってみました。

## CSSの基礎

いままでも簡単にCSSを使用してきましたが、
よくわからずにデタラメに書いてることも多々ありました。
それでは怒られてしまうので、
基礎をもう一度教わりました。

とくにCSS初心者が抑えておきたいこととしては以下の3点があります。

- インライン要素やブロック要素について
- floatの使用について
- CSSの優先順位について

以上の点は非デザイナーの方でもしっかり抑えておきたいところになります。

### インライン要素やブロック要素について

インライン要素とブロック要素についてはこちらにわかりやすく記載されています。

→[ブロックレベル要素とインライン要素について](http://www.tagindex.com/html_tag/basic/block_inline.html)

一般的なインライン要素は高さや幅を指定できませんが、
CSSでインラインブロックを使用すれば高さと幅も指定できるようになります。

→[CSSの display: inline、display: block、display: inline-block をマスターしよう！](http://taneppa.net/display-inline-block/)

### floatの使用について

たとえば、ある要素を右寄せにしたいとか左寄せにしたい、違う要素の右側にまわりこませたい
などのときにfloatが使用できます。

floatはその意味の通り対象の要素を「ふわっと浮かせる」効果があります。

詳しくはこちらを見るとわかりやすいかと思います。
→[CSSの【float】についてちょっと本気出して説明してみた。](http://taneppa.net/float/)

floatをうまく利用することができるようになれば、
Webサイトもつくりやすくなります。(慣れるのには修行が必要とのことなので頑張ります。)

### CSSの優先順位について

CSSには優先順位があります。
これらはポイント制になっています。
ただ、単なるポイント制というよりはバージョン番号っぽく考えるのがいいそうです。

→[エンジニアはもう一度CSSとちゃんと向き合ってみよう - 詳細度編](http://qiita.com/izumin5210/items/8ae78cb4f4bd325bccb4#a1)

これらの部分は非デザイナーでもしっかりと抑えておきたいところなので、
自分も使いまくって慣れていくようにします。

そしていよいよMaterial Design Liteの使用です。

## Material Design Liteとは

Material Design Liteは、
Googleが提唱するマテリアルデザインスタイルの
CSS/Javascript/HTMLのテンプレートとコンポーネントを含んだツールキットのことです。
これを使用することにより非デザイナーでもかっこいいデザインが簡単に使用できます。

例えばこんな感じのかっこいいテーブルもさくっと作成できます。

[!キャプション](http://www.yukihirai0505.com/wp-content/uploads/2015/07/ranking-300x177.png)

検索フォームなんかも簡単につくれます。

[!キャプション](http://www.yukihirai0505.com/wp-content/uploads/2015/07/search.png)

画像なのでダイレクトには伝わりにくいですが、
使い心地のいいサイトを簡単に作成できます。

Material Design Liteはこちらのサイトからダウンロードして使用することができます。
→[Material Design Lite](http://www.getmdl.io/)

サンプルサイトはこちらをご覧ください。
→[http://www.getmdl.io/templates/android-dot-com/index.html](http://www.getmdl.io/templates/android-dot-com/index.html)

ちなみに他にも似たような感じで、
いい感じのデザインを簡単に使用できるCSSフレームワークがたくさんあるので
気に入ったやつは使ってみたいです。

→[2015年も注目したいCSSフレームワーク6選](http://tech.eversense.co.jp/1023)
