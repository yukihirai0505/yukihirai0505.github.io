---
layout: post
title:  "gulpでPostCSSを使用してみる"
date:   2016-10-10 13:09:43 +0900
categories: Development
---

今回、同じ会社のエンジニアにgulpについて教えてもらったので、
忘れないように記事に残しておこうと思います。


## gulpとは

gulpとは[公式サイト](http://gulpjs.com/)を見てみると次のような説明があります。

> Automate and enhance your workflow

つまり、作業を自動化してくれたり効率的にしてくれるものです。
なんとなくフロントエンドまわりのタスクを自動化してくれるツール、
という認識はあったのですが特に使用してこなかったので実際に使うのは初でした。
フィリピンにいたときに、フィリピンのエンジニアにその存在を聞いてはいたのですが。笑

![キャプション](http://www.yukihirai0505.com/wp-content/uploads/2015/03/cropped-IMG_2828.jpg)

懐かしい。笑
実際使ってみて思ったのは

***「もっと早く使っとけばよかった」*** です。笑

Node.jsとnpmを使うのでこれらを環境に入れて使います。

インストールなどは公式のドキュメントをみて進めるのがいいかと思います。

→[Getting Started](https://github.com/gulpjs/gulp/blob/master/docs/getting-started.md)

## PostCSSを使ってみる

今回はPostCSSを使用してベンダープレフィックスを自動で付与するautoprefixer、
現在策定段階でブラウザが未実装のCSSの記法を、
今のブラウザが解釈できるようにトランスパイルするcssnext
入れ子でCSSを記述できるようになるpostcss-nested
ミックスインを使用するためのpostcss-mixins、
CSSで変数がつかえるようになるpostcss-simple-vars、
CSSをスペースや改行などを除いて縮小してくれるcssnanoなどを使用していきたいと思います。

ちなみにこれらはnpmでモジュールをインストールする必要があります。

[https://gist.github.com/yukihirai0505/2b03c9a41ce052252976](https://gist.github.com/yukihirai0505/2b03c9a41ce052252976)

## gulpfile.jsの記述

今回は下記のような感じでgulpfile.jsを記述してみました。

[https://gist.github.com/yukihirai0505/1375aeb81fcfe1d8514f](https://gist.github.com/yukihirai0505/1375aeb81fcfe1d8514f)

これで今回インストールしたモジュールを使用することができます。

今回は

`src/**/*.css`

こちらのパスのcssファイルを対象に実行しています。

cssの中身はそれぞれのモジュールを使ってみたかったのでこんな感じで記述してみました。

[https://gist.github.com/yukihirai0505/f7af05c87363e82621cb](https://gist.github.com/yukihirai0505/f7af05c87363e82621cb)

これでgulpを実行すると

こんな感じで生成してくれました。

[https://gist.github.com/yukihirai0505/2ef05756187d0e4968e8](https://gist.github.com/yukihirai0505/f7af05c87363e82621cb)

CSSで入れ子ができたり、変数が使えるようになると
かなり楽に記述していくことができます。
またベンダープレフィックス自動でつけてくれるとか、、、
なんて有難いんでしょう。
いちいち[CanIuse](http://caniuse.com/)で調べなくても良くなりそうですね。

他にもgulpにはwatchでファイルを監視してタスクを自動実行したりとか、

gulp-webserverを使用してlivereloadなどもできます。
面倒な処理を手軽に請け負ってくれるgulp、、、最高ですね。

ちなみにQiitaでも記事を書いてみました。

→[gulpで複数タスクの実行順序を指定したい場合](http://qiita.com/yukihirai0505/items/0e390514e780ef77342a)

ありふれてる簡単な内容なのでストックゼロです。笑
ただ、コメントしていただいた情報から
gulp4.0のドキュメントを見てみたんですが
gulpは今後もどんどん使いやすくなっていきそうです。
もうgulpから目が離せませんね。