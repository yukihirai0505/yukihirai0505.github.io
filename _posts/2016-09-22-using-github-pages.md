---
layout: post
title:  "Using GitHub Pages"
date:   2016-09-22 13:09:43 +0900
categories: Development
---

今回は[GitHub Pages](https://pages.github.com/)を使用して無料でWEB上でページを公開する方法について書いていきます。

## [GitHub Pages](https://pages.github.com/)とは

[GitHub](github.com)のリポジトリからダイレクトにホスティングしてくれるサービスです。
簡単に言うと、[GitHub](github.com)で管理しているコードをそのままWEBページとして公開することができるサービスです。

「[GitHub](github.com)って何や」って方は下記の記事を参考にどうぞ。

https://toiroha.jp/article/detail/41154

[GitHub](github.com)で管理しているソースコードがそのままWEBページに反映されるので、
ファイルを編集してadd・commit・pushすれば変更がそのまますぐに反映されます。

基本的に、静的なWEBページを公開するものなので
サーバーサイド言語は使用できない？ですがもちろんjsは使用できます。

## WEBページ公開までの手順

下記のリンクを参考に進めていきます。

https://pages.github.com/

###①リポジトリの作成

まずは[GitHub](github.com)でリポジトリを作成します。

- [リポジトリの作成](https://github.com/new)

リポジトリを作成するときは

`[ユーザーネーム].github.io`

という名前で作成します。

###②リポジトリをクローン

次に作成したリポジトリをクローンします。
コマンドラインで
`git clone`するもよしzipを落とすもよし。

###③index.htmlファイルの追加

次に `index.html` ファイルを追加します。

今回はチュートリアルの内容と同じものを作成します。

```
index.html
<!DOCTYPE html>
<html>
<body>
<h1>Hello World</h1>
<p>I'm hosted with GitHub Pages.</p>
</body>
</html>
```

ファイルが作成できたら

```
git add index.html
git commit -m "Add index.html"
git push origin master
```

とかでファイルをリモートリポジトリにプッシュしてあげます。
これで

今回作成した `http://[ユーザーネーム].github.io` というURLにアクセスすればページが公開できていることが確認できるかと思います。

##最後に

自分のリポジトリと公開ページのリンクを下記に記載しておきますのでわからない方は参考にしてみてください。

- [リポジトリ](https://github.com/yukihirai0505/yukihirai0505.github.io)
- [公開ページ](https://yukihirai0505.github.io/)
