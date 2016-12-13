---
layout: post
title:  "CometとAkka Actorを使用してサーバーから定期的にブラウザにPushする"
date:   2016-10-04 13:09:43 +0900
categories: Development
---

今回は個人的にPlayframework2.4を使用して

Comet × Akka Actorでサーバーから定期的にブラウザPushする方法を試したので簡単にまとめてみます。

定期的にブラウザPushを行うことに重点を置くため、
できる限りシンプルに実装していきます。

## Cometとは

Cometとはリクエストをサーバーに送信後タイムアウト、またはサーバイベントが発生するまで
長時間リクエストを存続させておくWEBアプリケーションモデルです。
すごく簡単に言えば、サーバーからクライアントに情報をpushできるものです。

詳しくはこちらをどうぞ

↓

[https://ja.wikipedia.org/wiki/Comet](https://ja.wikipedia.org/wiki/Comet)

## Akka Actorとは

Akka Actorは並列処理のためのもので、
詳しくはこちらのスライドで紹介されています。

- [http://www.slideshare.net/sifue/akka-39611889](http://www.slideshare.net/sifue/akka-39611889)
- [http://www.slideshare.net/TakamasaMitsuji/ss-13626473?related=1](http://www.slideshare.net/TakamasaMitsuji/ss-13626473?related=1)

今回はCometからのpushを繰り返し行うために使用しています。

## 実際に実装してみる

流れとしては

- ①Userの実装(別にUserでなくても良い)
- ②Actorの実装
- ③Controllerの実装
- ④Viewの実装

といった感じになります。

## Userの実装(別にUserでなくても良い)

まずはカウントをもつUserを作成します。
本来はここにちゃんとしたmodelのUserを置きたいところですが、今回はめちゃめちゃさっぱりにします。

{% gist yukihirai0505/ce75b97f80d4fab1a803 %}

## Actorに渡す用のクラスを作成

次にCometオブジェクトをもつActorに渡す用のクラスを作成します。

{% gist yukihirai0505/2d3d4774cd89b7c57fc9 %}

## Actorの実装

次にActorを実装します。

{% gist yukihirai0505/87a9fc69c9b5d85e8f7f %}

## Controllerの実装

ここまで出来たら次はControllerを実装していきます。

{% gist yukihirai0505/7d4e68839f8f32baf340 %}


## Viewの実装

最後にviewを作成してあげます。

{% gist yukihirai0505/ddb872a3a4a35d51461d %}

これで
forever iframeによりサーバーからpushがある度にiframeの中身が書き換わって

{% gist yukihirai0505/cb0718222b0c27f8be12 %}

ここの部分でブラウザを動的に書き換えています。

## まとめ

これらを応用すればチャットが作れたり、
テーブルの値が変更するまでユーザーを待機させておくなんていうことが可能になります。

Githubのコードもあるので良かったらどうぞ！

[https://github.com/yukihirai0505/comet_actor](https://github.com/yukihirai0505/comet_actor)

変なところがあればプルリクもお待ちしてます^^
