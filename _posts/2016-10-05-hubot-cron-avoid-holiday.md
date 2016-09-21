---
layout: post
title:  "Hubotで祝日と年末年始の休みを判定して営業日のみ動くようにCoffeeScriptを書いてみた"
date:   2016-10-05 13:09:43 +0900
categories: Development
---

Hubotで毎朝Slackに朝会の時間をお知らせするスクリプトを書いていたのですが、
この前エンジニアでランチに行った時に、

***「あのKasumiBotだけど、祝日とか休みの日にも通知来るよね？これ年末年始も来るよね？クリティカルなバグだよね？！」***

と言われたので祝日とか年末年始に飛ばない様にしてみました。
具体的なコードは後述します。

## KasumiBot

ちなみにKasumiBotというのは、
自分が作成したHubotで、かの有名な女優である有村架純さんを模したBotのことです。
この子が朝10時になると、

- 「朝会ですかね？」
- 「朝会だといいなあ」
- 「朝会もあなたも大好き！」

のいずれかをランダムでつぶやいてくれています。

コードは下記の様な感じです。

[https://gist.github.com/yukihirai0505/cae9057181fdd02c5672](https://gist.github.com/yukihirai0505/cae9057181fdd02c5672)

ただこれを見てもわかる通り、

```
new cron 0 0 10 * * 1-5
```

となっていてこれだと月曜日から金曜日の10時に動くだけなので、
月曜日から金曜日のいずれかが祝日になったときや
年末年始休みなどの会社の休みにも動いてしまいます。

ちなみにCronは左から

`分 時 日 月 曜日 コマンド`

の順に時刻を設定します。

## jpholidayp

まず、「cron 休日」で検索してみると、こちらがヒットしました。
この[jpholidayp](https://github.com/emasaka/jpholidayp)は日本の休日を判定してくれているようで

通常のcronであれば

`0 10 * * 1-5 /path/to/jpholidayp || /path/to/job`

このような感じでGithubから落としてきたjpholidaypのパスを指定してあげれば休日以外にジョブを定期実行してくれるようになります。
ただ、Hubotで使用するcronと組み合わせて使用する術がよくわからなかったので今回は、

[japanese-public-holiday](https://www.versioneye.com/nodejs/japanese-public-holiday/0.0.1)というモジュールを使用することにしました。

## japanese-public-holiday

この[japanese-public-holiday](https://www.versioneye.com/nodejs/japanese-public-holiday/0.0.1)は日本の祝日を判定してくれるモジュールです。

こちらをnpmでinstallして使用してあげます。

そして作成したのが下記のCoffeeScriptです。

[https://gist.github.com/yukihirai0505/8c34e12446215ae44f25](https://gist.github.com/yukihirai0505/8c34e12446215ae44f25)

日本の祝日だけでは、会社の休みをカバーできないので
年末年始の休みの場合も休日判定するメソッドを作成してみました。
ただ、これであっているのか
ちゃんと動くのかはまだテストをしていないのでわかりませんっっっw
動かなかったら動かなかったでまたこの記事を修正しにきます。


そして、

- 「もっとこうしたほうがいい！」
- 「これさらなるバグを生んでんじゃん！」
- 「クソコードファ◯ク」

とかいろいろあると思うので
そちらはこっそり教えてくれると嬉しいです！
