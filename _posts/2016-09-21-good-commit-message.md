---
layout: post
title:  "Good commit message"
date:   2016-09-21 13:09:43 +0900
categories: Development
---

今回はGitのコミットメッセージについて記事にしてみます。

[https://github.com/erlang/otp/wiki/Writing-good-commit-messages:title]
という記事があったので、自分なりに日本語に変換して理解しておきます。

## Writing good commit messages

まずは簡単に日本語でまとめてみます。


### 良いコミットメッセージは少なくとも3つの重要な目的がある。

1. レビュープロセスをより早く行う
2. わかりやすいリリースノートを書く手助けとなる
3. 5年後など将来のメンテナンスのため(なぜそのような変更をしたのか、なぜその機能が追加されたのか)

コミットメッセージは次のような形が望ましい。

50文字以下の短い要約。

必要であればより詳細に。
72文字程度におさめる。
最初の行は、メールでいう件名。
残りのテキストはメールでいう内容。
空行のラインで要約と内容を区別する。

それ以降の段落は空行のあとに書く様にする。

重要なところはハイフンやアスタリスクを使う。
シングルスペースと空行を間にはさむ。

### ルール

- "Fixed","Added","Changed"のような過去形ではなく最初に"Fix","Add","Change"のように書くように統一する
- 常に2行目は空行にする
- 要約の最後にピリオドを使用しない(タイトルにピリオドは使用されない)

### ヒント

コミットを要約するのは難しい様に思えるけど、
それは多分複数のロジック変更やbugfixを含んでいるから。
コミットは分割しよう!

## まとめ

プロジェクトごとによってコミットの書き方はそれぞれ異なってくると思う。
今回のリンク先のプロジェクトはこのルールに従ってみんなコミットするようにしているよう。

以下、例


```
dialyzer: Fix a bug concerning the option 'plt_remove'

[James Fish:]
Dialyzer always asserts that files and directories passed in its
options exist. Therefore it is not possible to remove a beam/module
from a PLT when the beam file no longer exists. Dialyzer should not to
check files exist on disk when removing from the PLT.
```

```
tools: Fix wrong instrumentation of binary comprehensions

When cover instruments binary comprehensions it's generating a
{block, ...} abstract code term inside a {bc, ...} term that is causing
the evaluation to fail at runtime. Removing the block statement
eliminates the error.

The template of a bit string comprehension cannot have a counter since
it is not allowed to be a block.
```

プロジェクトによってコミットメッセージにルールがあると見やすくなるので
意外とこういうルール決めは重要かもしれない。
日本語や英語が入り混じったりすると読むのがしんどいかもしれない。


- できるだけ簡潔に
- わかりやすく


を心がけてメッセージを残していきたいものです。
