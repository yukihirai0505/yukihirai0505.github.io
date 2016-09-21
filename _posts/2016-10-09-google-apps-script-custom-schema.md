---
layout: post
title:  "Google Apps ScriptでcustomSchemaの作成と更新"
date:   2016-10-09 13:09:43 +0900
categories: Development
---

先週は、社内で既存サービスの新機能開発に取り組み
一通りの機能を実装してリリースしました。
そして今週から社内システム開発用にGoogleAppsScriptを触り始めています。


## Google Apps Scriptとは

GoogleAppsScriptとは

> Googleが提供するサーバーサイド・スクリプト環境です。
Googleスプレッドシート、Gmail、Googleカレンダー、Googleマップ、Googleサイト等々、
Googleが提供するあらゆるサービスを統合的に処理する一大スクリプト環境なのです！([SYODA-tuyano](http://libro.tuyano.com/index2?id=638001)より引用)

簡単に言えば、GoogleAppsScriptというものを使用すれば、
Googleが提供しているサービスがより便利に使用できるようになりますよ、というものです。
例えば、面倒なルーチンワークを自動化したりできます。
ちなみに、GoogleAppsScript≒Javascriptのような感じになっていて
略してGASって呼ぶそうです。
今回は主に組織管理で必要そうなcustomSchemaについて書いていきます。

## Admin Sdkの使用

組織管理するにあたってAdmin Sdkというものを使用していきます。
詳しくはこちらのドキュメントをご覧ください。
→[https://developers.google.com/admin-sdk/](https://developers.google.com/admin-sdk/)

最初のところにもありますが、

> Manage your domains and apps with the Admin SDK.

とあるように、組織管理を便利にしてくれるものです。

## Directory APIの使用

上記のAdmin Sdkの中にあるこのDirectory APIを使用して、
組織管理を行っていくことができます。
→[https://developers.google.com/admin-sdk/directory/](https://developers.google.com/admin-sdk/directory/)

> The Directory API lets you perform administrative operations on users, groups, organization units, and devices in your account.

こちらにもあるように、このAPIを使用することで
ユーザーの追加・削除であったり、グループの作成などなど他にもいろんなことができます。

## customSchemasの作成

さて、今回はUserの情報に含まれているcustomSchemasの作成を行っていきます。
ちなみにcustomSchemaとはここにもあるように

> You can define custom fields for users on your domain by adding custom user schemas to the domain. You can use these fields to store information such as the projects your users work on, their physical locations, their hire date, or whatever else fits your business needs.

→[https://developers.google.com/admin-sdk/directory/v1/guides/manage-schemas](https://developers.google.com/admin-sdk/directory/v1/guides/manage-schemas)

簡単に言うとUser情報にある程度自由に情報を保持させておくことができるようなものです。
作成して情報を取得するまでの順序としては下記のような流れになります。

- Shemaを作成
- customSchemasを更新
- User情報からcustomSchemasを取得

## ①Schemaの作成

まずはSchemaの作成を行います。
コードは下記のような感じになります。

[https://gist.github.com/yukihirai0505/d4c8168e91420b00ef25](https://gist.github.com/yukihirai0505/d4c8168e91420b00ef25)

Schemaはデータベースでいうテーブル名みたいなもので、
その中身がカラム名のような感じになっています。
これでemplymentDataというSchemaを作成することができました。
ちなみにコードからもわかりますが、Schemaを取得する際は第二引数にSchemaKeyを渡してあげれば、
指定したSchemaを取得することができます。

## ②customSchemasを更新

さて、次に作成したSchemaの情報をUserの方で更新していきます。
コードは下記のような感じになります。

[https://gist.github.com/yukihirai0505/727b5ec2d5a31ccc8aa1](https://gist.github.com/yukihirai0505/727b5ec2d5a31ccc8aa1)

これで指定したユーザーのcustomSchemaの更新ができます。
今回は先ほど作成したemploymentDataに値を保存しています。

## ③User情報からcustomSchemasを取得

最後に今回作成した情報をUserから取得する方法を見ていきます。
ドキュメントを読んでみると下記のような記載があります。

> Read custom fields in a user profile
You can fetch custom fields in a user profile by setting the projection parameter in a users.get or users.list request to custom or full.

ここにもある通り、customSchemaで指定した値を取得するには
User情報を取得する際に `full` や `custom` などのオプションを指定してあげる必要があります。

コードに落とすと下記のような感じになります。

[https://gist.github.com/yukihirai0505/ee8035c07d5c978d7099](https://gist.github.com/yukihirai0505/ee8035c07d5c978d7099)

このように `custom` で指定する際は
`customFieldMask` でSchemaを指定してあげる必要があります。

今回は主にcustomSchemaについて書いてみましたが、
なにせまだGoogleAppsScriptを触り始めて数日なので
間違いがあればコメント等していただけると幸いです^^

先ほども紹介しましたが、
今回の内容は下記URLから確認いただけます。
[https://developers.google.com/admin-sdk/directory/v1/guides/manage-schemas](https://developers.google.com/admin-sdk/directory/v1/guides/manage-schemas)

少しでも早くGASに慣れていきたいです。

## SchemaをmultiValued: trueで登録した場合

Schemaを `multiValued:true` で保存した場合には
下記のドキュメントを参考に値を保存しましょう。
→[https://developers.google.com/admin-sdk/directory/v1/guides/manage-schemas#set_fields](https://developers.google.com/admin-sdk/directory/v1/guides/manage-schemas#set_fields)

[https://gist.github.com/yukihirai0505/b4a40b1fe1ad52597b36](https://gist.github.com/yukihirai0505/b4a40b1fe1ad52597b36)

> Single-valued custom fields use the "field1": "value1" format. Multi-valued fields are similar to other multi-value fields, such as addresses where individual values can be tagged as standardType with allowed values custom, home, work, or other or as a customType

`multiValued:true` のときの値の設定に気をつけましょう。
キーが固定になっていて、typeは指定できる値もcustom, home, work, or otherなどに限られます。

> If a custom field in a schema is not specified at update time, it is left unchanged.

更新時にフィールドを指定しないとそのままに更新されないのでこちらも注意ですね。

【補足】
結局、今回はいろいろ検討した結果GASは使用せずにAPIをJavaから叩くことになりそうです。笑
たった3日の付き合いだったけどありがとうGAS。
