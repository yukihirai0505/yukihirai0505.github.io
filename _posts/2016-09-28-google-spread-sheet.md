---
layout: post
title:  "GoogleスプレッドシートとGoogleAppsScriptでアナリティクスのレポートを自動化"
date:   2016-09-28 13:09:43 +0900
categories: Development
---

今回、Googleスプレッドシート上でアナリティクスのデータを管理していたので
GoogleAppsScriptでその作業を自動化してみました。

当初、レポート業務を引き継いだとき、
いちいちアナリティクスを開いて期間を選択して結果をコピペして貼り付ける。
これが日毎、週ごと、月ごととなるので
多い時になると週明けが月初のときはGoogleアナリティクスのページをぽちぽちしまくらねばなりません。

それが非常に面倒くさいのでGoogleAppsScriptを使用して自動化することにしました。

## 自動化の方法

まず、自動化の方法ですがいくつかあるみたいです。

- アドオンを使用する方法

[http://tonari-it.com/google-analytics-spreadsheet/](http://tonari-it.com/google-analytics-spreadsheet/)

- Google Apps Scriptを使用する方法

[http://qiita.com/sebisawa/items/1ae2f871fb8ff3366064](http://qiita.com/sebisawa/items/1ae2f871fb8ff3366064)

前にGoogle Apps Scriptは触ったことがあったので、
今回は後者のAppsScriptを使用する方法でいきます。

[http://yukihirai.hatenablog.com/entry/%3Fp%3D2769](http://yukihirai.hatenablog.com/entry/%3Fp%3D2769)

## Google Analytics API用のスクリプト

まず、スクリプトエディタ上でGoogleAnalyticsAPIをONにして
次にコードを書いていきます。

{% gist yukihirai0505/80e75c5493698a85f927 %}

上記のコードではpathを指定して、
全体の結果とpath毎の結果を取得してスプレッドシートにデータをセットしてくれるメソッドです。

## 日付を扱うスクリプト

また、今回日付を扱うことが多かったのでその処理をまとめておきました。

{% gist yukihirai0505/5d23464ba18213628ead %}


## 日別/週別/月別で実行する

あとは、上記のメソッドを用いて日別/週別/月別で実行するスクリプトを書いていきます。

{% gist yukihirai0505/be0716571902d23c2600 %}

これは週毎に実行されるスクリプトですが、
これと同じようなものを日別・月別でつくってあげます。

## トリガーの設定

最後にトリガーで定期実行する処理を書いてあげます。
トリガーはGUIからも作成・削除できますが
コード上で管理しておくようにしてみました。

{% gist yukihirai0505/b7d49b37d7a23266a0af %}

上から順に、

- 毎日10時に実行
- 毎週月曜日の10時に実行
- 毎月1日の10時に実行

といった形になります。

これで書いたスクリプトが自動実行されるようになります。

ここまでしておけば今後のGoogleAnalytics関連の処理は自動で行われるので、
自分が何もしなくても
「おー、平井君は毎日データ集計しててえらい！」
という嬉しい結果を得ることができます。

入力ミスもなくなりますし、いいことずくめですね。

コードに関してはノンストップで一気に書いていったので、
修正点があればgistでコメントいただけると嬉しいです^^

随時修正していきたいと思います！

[https://gist.github.com/yukihirai0505](https://gist.github.com/yukihirai0505)

