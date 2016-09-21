---
layout: post
title:  "仕事でMySQLを使い始めて知っとけばよかったなと思うこと"
date:   2016-10-06 13:09:43 +0900
categories: Development
---

今までORMに頼り切りであまりSQLをどっぷり触る機会がなかったのですが
ここ2~3週間でSQLを触る機会が前の1000倍ほど増えたので備忘録も兼ねてMySQLについて書いていきます。
もともとSQL自体はエンジニアになる前から書いてはいたんですが、その頃知らなかったかなり基本的なところも書いていくので、SQL中・上級者の方は何も得るものはないかと思います。笑
むしろ、アドバイス頂けると嬉しいです！

## 簡単なSQLの書き方

### テーブル名は短く別名をつける

これは個人によって違うかもしれませんが、
user uなどのようにテーブル名は省略して書いています。

[https://gist.github.com/yukihirai0505/f2771898c7ad7bb6e9ff](https://gist.github.com/yukihirai0505/f2771898c7ad7bb6e9ff)

といった具合です。
こう書いておくと後から見返したりチューニングする時もすっきりしているので良いですね。

## CASE式

このCASE式も結構使います、条件によって表示するものを変えたいとかいった場合に使用したりします。

[https://gist.github.com/yukihirai0505/695025bc2df53ff2e213](https://gist.github.com/yukihirai0505/695025bc2df53ff2e213)

といった書き方もできます。

### ORDER BYを使用して複数条件で並び替えする方法


`ORDER BY s.start_date desc, s.id desc`

### GROUP BYもORDER BY同様に複数条件を同時に指定

`GROUP BY s.start_date desc, u.id`

### 部分置換

[https://gist.github.com/yukihirai0505/749deecc8fb6f9909817](https://gist.github.com/yukihirai0505/749deecc8fb6f9909817)

## チューニング編

最適化するべきクエリはスロークエリログやクエリアナライザで見付けられます。
ここのブログで紹介されてるやり方で、
スロークエリの設定が確認できます。
[Mysql slow queryの設定と解析方法](http://d.hatena.ne.jp/masayuki14/20120704/1341360260)

MySQLをbrewで入れている場合は

`ls $(brew --prefix mysql)/support-files/my-*`

とかでファイルを確認してみて、そこにあれば

`sudo cp $(brew --prefix mysql)/support-files/my-default.cnf /etc/my.cnf`

とかでコピーしておくのが良いです。

ここで上がってきたクエリーは要チューニングです。

### EXPLAIN


データのチューニングで使用するEXPLAINは、
クエリーの前にEXPLAINと付け加えれば、その実行結果を表にして示してくれます。
ただ、初見でこの表を見てもあまり何を言っているかわからないかと思います。

このEXPLAINの結果に関しては、
次のブログで詳しく紹介されています。
[MySQLのEXPLAINを徹底解説!!](http://nippondanji.blogspot.jp/2009/03/mysqlexplain.html)
表の各項目についても上記のブログで紹介されてますが、ひとまずチェックしたい部分としてtypeのカラムがあります。

typeの部分が、
indexまたはALLの場合はチューニングの余地がありそうです。

そこからALLのものは
場合によってですが、対象カラムにインデックスを貼ってあげるなどすると良いです。

[https://gist.github.com/yukihirai0505/2234462dc5858e2c27b9](https://gist.github.com/yukihirai0505/2234462dc5858e2c27b9)

<h2>◯◯分単位でログを集計したいなど</h2>

[https://gist.github.com/yukihirai0505/a8c1e31ec1d453e868d4](https://gist.github.com/yukihirai0505/a8c1e31ec1d453e868d4)

SETで変数を指定できます。

繰り返し使用するようなものは最初にSETしておくと良いですね。
ちなみに

`UNIX_TIMESTAMP`

日付をunixtime(UTCの1970年1月1日0時0分0秒からの経過秒数)に変換してくれる。

`FROM_UNIXTIME`

unixtimeで格納されている時間を適当なフォーマットで返すことのできる関数。
です！

## UNIONでのソート編

AのテーブルとBのテーブルをUNIONする時、
Bのテーブル内でorder byをしていても反映されないことがあります。
Bのテーブル内でソートしたものを更にセレクトして別テーブルで定義すればソートが効いた状態でUNIONできます。


[https://gist.github.com/yukihirai0505/f30095bd5d907e49ef2a](https://gist.github.com/yukihirai0505/f30095bd5d907e49ef2a)

## トランザクションの理解

> トランザクションとは、複数のユーザーが同時にデータベースを操作する状況において、データベースに対する複数の操作(選択、更新、削除)などを実行している途中で仮にエラーが発生したとしても、データの不整合が起きないことを保証するリレーショナルデータベースシステム(RDBMS)のメカニズムです。
詳しくは下記がわかりやすいので参照してみてください！

[トランザクションでデータの不整合を防ぐ](http://www.atmarkit.co.jp/ait/articles/0210/24/news001.html)

### SELECT...FOR UPDATE

トランザクションで、
SELECT FOR UPDATEクエリーを用いると、トランザクション終了までSELECTされた行に対して排他ロックが適用されます。

## CSVファイルのインポート

このCSVファイルのインポートにはいくつかトラップがありました。
まず、DBに読み込みたいCSVデータを用意します。
次に、MySQLに入っていくのですが、
入るときに

`mysql --local_infile=1 -uroot -p`

で、 `--local_infile` をONにしてあげます。

これを忘れると、

```
ERROR 1148 (42000): The used command is not allowed with this MySQL version
```

みたいなエラーが出ることがあります。

次に、

[https://gist.github.com/yukihirai0505/76fff4aa6a4ada62be0c](トランザクションでデータの不整合を防ぐ)

としてあげます。

ただ、ファイルによっては改行コードがCRのものがあります。
その場合これでインポートすると、1行分しかインポートされないということが起こりえます。
なので改行コードをCRからLFに変換すれば解決します。
お使いのテキストエディタで変換するのもいいですが、
nkfコマンドという便利なコマンドがあるのでそちらを使用しましょう。
Macの方は

`brew install nkf`

とかでサクッとインストールしちゃいましょう。

次にnkfを使用して対象ファイルがCRもしくはCR+LFの改行コードだった場合は、

`nkf -Lu [対象ファイル] > [変換後のファイル名]`

これでオッケーです。
これで変換すると改行コードが `\r` から `\n` に変換されているのがわかります。

便利ですね。
変換が完了すればそのファイルはちゃんと全件インポートできるかと思います。


## selectした結果をinsertする

これも便利なので是非知っておきたい。

[http://qiita.com/ques0942/items/acfdd5382c638580ce0b:embed:cite]

## 最後に

まだまだMySQL知らないことばかりで、
効率が悪いものを書きがちですが
少しでも良いSQLが書けるように努めていきます。