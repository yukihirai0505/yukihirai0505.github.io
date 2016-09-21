---
layout: post
title:  "SNSから送るメッセージをLambda経由でSlackへ"
date:   2016-10-01 13:09:43 +0900
categories: Development
---

開発者向けのエラーメッセージを、SNSを使用して開発者のメーリス宛に送っていたのですが、
「Slackでも受け取りたいよね。」
ということで、Slackで受け取れるようにしました。

ちなみにSNSというのはAWSのサービスで、
正式名称をAmazon Simple Notification Serviceといいます。

[https://aws.amazon.com/jp/sns/](https://aws.amazon.com/jp/sns/)

## Slackに通知するまで流れ

下記のQiitaの記事でわかりやすく流れを説明してくださってます。

[http://qiita.com/tmtysk/items/7161b11e20ac5e2dfc01](http://qiita.com/tmtysk/items/7161b11e20ac5e2dfc01)

この流れに沿って

- SlackでIncoming WebHooksを作成
- Lambdaの作成
- テスト
- SNSで連携

といった感じになります。

## Lambdaでfunctionを作成してテスト

上記の記事に補足する形で説明すると、
まずはAWSコンソールに入ってLambdaを選択後
"Create a Lambda Function"をクリックします。
次にblueprint選んでねと言われますが、ここはSkip
次に言語はNodeJSを選んでコードを記述していきます。

コードに関しては、Qiitaの記事で紹介されているコードでうまくいかない場合があるかもしれません。
その場合は、
自分が書いたものをGistにあげたのでこちらを試してみていただくといいかもしれません。

[https://gist.github.com/yukihirai0505/0af52c9c98cc998a10a3](https://gist.github.com/yukihirai0505/0af52c9c98cc998a10a3)

基本、うまくいかない場合はLog見ながらデバッグしていけばだいたい解決するかと思います。
自分は最初そのまんまコピペしたら下記のエラーがでたりしたので、
console.logとかをうまく使いながらオブジェクトの中身確認していけば大丈夫かと思います。

- Process exited before completing request
- SyntaxError: Unexpected token a at Object.parse (native)

作成できたら次はテストをしていくのですが、
TESTをする際はSNSを選択します。
これでうまくいけば、Slackに通知が行きます。

## SNSで連携

次にSNSからTopicsを選択して、
今回送り先に指定したいLambda用のSubscriptionを作成します。
その際に今回作成したLambdaのEndpointを指定してあげれば無事連携完了です。

連絡ツールが複数になると、
こっちもあっちもチェックで大変ですがSlackで受け取れるものはSlackに通知してあげると手軽に一箇所で確認できていいですね。
