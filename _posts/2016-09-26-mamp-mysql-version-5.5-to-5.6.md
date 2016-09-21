---
layout: post
title:  "MAMPのMySQLのバージョンを5.5から5.6にあげたいとき"
date:   2016-09-26 13:09:43 +0900
categories: Development
---

今回、MAMPを使ってる人のPCで
MySQLのバージョンが違うためにエラーがでていたので5.5から5.6にする機会がありました。

簡単な流れをメモ代わりに記述しておきます。

## 便利なスクリプトを使ってみる

"MAMP MySQL 5.6"

とかで検索すると外国の方が良さそうなスクリプトを用意してくれていました。

リンクはこちら

[https://gist.github.com/tobi-pb/b9426db51f262d88515c](https://gist.github.com/tobi-pb/b9426db51f262d88515c)

コメント欄で結構いろいろ議論があるので随時各環境に応じて最適化されていくかと思います。

[https://gist.github.com/tobi-pb/b9426db51f262d88515c](https://gist.github.com/tobi-pb/b9426db51f262d88515c)


ただ、これをDownloadして実行する前に
wgetなど必要そうなコマンドは自分でインストールしておきましょう。


[http://d.hatena.ne.jp/replication/20150214/1423879393](http://d.hatena.ne.jp/replication/20150214/1423879393)


あとは落としてきたシェルスクリプトを実行するだけです。

```
sh migrate.sh
```

## エラーなどがでたときの対処

もしかすると上記のシェルスクリプトは使用環境によってはエラーで終わってしまうかもしれません。
ただ、2016/05/13時点では
このシェルスクリプトは要所要所でechoを使用して
行ってる処理を表示してくれているのでエラーが起きたところで実行されている処理を個別で抜き出して再実行してエラーの原因を突き止めるのがよさそうです。

- 各フェーズ
    - "stopping mamp"
    - "creating backup"
    - "copy bin"
    - "copy share"
    - "fixing access (workaround)"
    - "starting mamp"
    - "migrate to new version"

便利なものをつくってくれていて非常に助かります。
おかげさまで作業効率もあがりますね。
