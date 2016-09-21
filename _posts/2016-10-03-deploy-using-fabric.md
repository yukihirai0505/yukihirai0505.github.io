---
layout: post
title:  "Fabricで幸せなデプロイライフ"
date:   2016-10-03 13:09:43 +0900
categories: Development
---

自分のサーバでは現時点で、
Playframework2.4JavaとScalaがそれぞれ動いていて
デプロイを自動化する際にfabricを使用したので備忘録も兼ねてここに記します。

## Fabricとは

Fabricは

- コマンドライン 経由で 任意の <strong>Python 関数</strong> を実行するツールです。
- (低レベルライブラリの上に構築された)サブルーチンのライブラリで、SSH経由で***簡単に***かつ***Python風に***シェルコマンドを実行します。([概要とチュートリアルFabric ドキュメント](http://fabric-ja.readthedocs.org/ja/latest/tutorial.html)より引用)

つまり、このFabricを使えばいちいちサーバーに手動でアクセスして
作業せずともデプロイのようなタスクを自動化できるのです！

例えば下記のようにyum_updateというメソッドを作成したファイルを用意します。

[https://gist.github.com/yukihirai0505/cd46406c76e578c6d773](https://gist.github.com/yukihirai0505/cd46406c76e578c6d773)

そうするとこのメソッドを次のように叩くことができます。

[https://gist.github.com/yukihirai0505/f7e4057692bf980f0c61](https://gist.github.com/yukihirai0505/f7e4057692bf980f0c61)

これを叩けばリモートサーバー上でsudo yum updateを実行してくれるのです！

## Fabricの設定

上記のコマンドではユーザ名,sshの秘密鍵,ホスト名を指定して
コマンドを叩いていますが、
これはファイル上に書いておくこともできます。
今回、自分のサーバー対して作ったものを簡単にしたものが下記になります。

[https://gist.github.com/yukihirai0505/5964130f78d6fc09414e](https://gist.github.com/yukihirai0505/5964130f78d6fc09414e)

ここの下記の部分で設定を書いておくことができます。

```
env.hosts = ['ホスト名']
env.user = 'ユーザー名'
env.key_filename = '鍵の場所'
```

そうすることで

`fab [実行したいメソッド]`

このようにシンプルにコマンドを叩くことができます。

この中身を見ていくと、

サーバー上の処理だけでなくローカルでもコマンドを叩いて
ファイルの圧縮をしてくれているのがわかります。

ローカル上での処理には
lcd()や
local()を使用しているのがわかります。

ローカルで圧縮したファイルを
put()
でサーバー上に送っています。

またサーバー上でも動いているプロセスを止めたり、
ビルドコマンドを走らせたりといったこともできます。

基本サーバ上でコマンド叩く場合は
run()
を使用しています。

## デプロイ自動化の幸せ

このFabricを使えばデプロイ処理をお任せできます。
上記のファイルでいくと

`fab deploy`

たったこれだけのコマンドです。
めちゃめちゃ便利ですね。

皆さんもFabricを使用して幸せなデプロイライフを送ってみてください^^
