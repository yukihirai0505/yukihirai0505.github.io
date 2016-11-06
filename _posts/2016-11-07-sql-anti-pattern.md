---
layout: post
title:  "SQLアンチパターン データベース論理設計のアンチパターン"
date:   2016-11-07 12:00:00 +0900
categories: Development
---

本日もSQLアンチパターンを読んだので、
出てきた用語やアンチパターンを一部抜粋してまとめてみます。

## ちょいちょい出てくる名言まとめ

> Ihr seid alle Idioten zu glauben, aus Eurer Erfahrung etwas lernen zu konnen, （諸君は自らの経験からいくらか学ぶことができるという，全く愚かな考えであろうが，）
  ich ziehe es vor, aus den Fehlern anderer zu lernen, um eigene Fehler zu vermeiden.（余はむしろ他人の失敗を学ぶことで，自分の失敗を回避することを好む）
  qt: http://okanos.com/2012/02/10202432.php

> An expert is a person who has made all the mistakes that can be made in a very narrow field.
  by ニールス・ボーア

> One in a million is next Tuesday
  by Gordon Letwin

## ①ジェイウォーク(信号無視)

開発者はよく「多対多」の関連を表現する ***交差テーブル*** の作成を避けるために
カンマ区切りのリストを使用する。
これをジェイウォーク(信号無視)と呼ぶ。

"intersection"(交差点/交差テーブル)を避けようとする行為だから。

### デメリット

パフォーマンス低下やデータ整合性の問題

- 等価性による比較ができないためなんらかの文字列パターンにパターンマッチが必要
- パターンマッチは式の書き方によって意図しない結果を返す可能性がある
- インデックスを使用するメリットも得られない
- 結合(JOIN)するのに手間がかかり、効率的なものにならない
- 集約クエリ(COUNT, SUM, AVG)を使用する際に開発に時間がかかりデバッグも難しくなる
- アカウントの更新が冗長になる
- アカウントIDのバリデーションが難しい
- 個々の入力値に区切り文字がある場合がやっかい
- 長さに制限がバラバラ

### アンチパターンを用いてもいい場合

データベース構造に ***非正規化(denormalization)*** を適用する場合
ただし、十分な検討が必要。
正規化されていたほうが柔軟性は高くなり、データの整合性を保つ助けにもなる。

## ②ナイーブツリー(素朴な木)

ニュースサイトで各スレッドにコメントができるような場合。
コメントをツリー構造で表したとき
各エントリは ***ノード*** と呼ばれる。
ノードは1つ以上の子と、1つの親を持てる。
親を持たない最上位のノードは根(ルート)
子を持たない最下位のノードは葉(リーフ)
中間のノードは非葉ノード

ツリー指向のデータ構造の代表例

- 組織図
- スレッド形式のコメント欄

`parent_id` 列を加えるという発想だと思慮が浅い、ナイーブと言える。
`parent_id` は同じテーブルの別の行を参照する。

このような設計は隣接リスト(Adjacency List)と呼ぶ。

### デメリット

- すべての子孫を取得するクエリを書くのが冗長になる
- COUNTのような集約関数の扱いが難しくなる
- ノードの削除の際最下層から順に削除しなければならない(ON DELETE CASCADEで自動化できるがノードの昇格や移動は自動化できない)

### アンチパターンを用いてもいい場合

- ノードの直近の親と子の取得のみ
- 列の挿入が容易

階層構造を持つデータに大して必要な操作がこれらのみ、といった場合は効果的。
またSQL拡張機能を備えている製品もある。 *再帰クエリ*

***解決案***

代替ツリーモデルを使用

- 経路列挙モデル(Path Enumeration)
- 入れ子集合モデル(Nested Set)
- 閉包テーブルモデル(Closure Table)

### 経路列挙モデル(Path Enumeration)

隣接リストには先祖ノード取得の効率が悪い弱点がある。
先祖の系譜を表す文字列を各ノードの属性として格納することで問題を解決する。
`parent_id` の代わりに文字列で `path` 列を定義する。

ただしジェイウォークで示した弱点がある。
DBはパスの正確な形状や、パスの値の既存ノードへの対応を保証できない。

### 入れ子集合モデル(Nested Set)

直近の親ではなく、子孫の集合に関する情報を各ノードに格納する。
この情報は、各ノードの `nsleft` や `nsright` と呼ばれる数値で表される。

nsleft...そのノードより下にある階層にあるすべてのノードがもつ値より小さな値がはいる
nsright...そのノードより下の階層にあるすべてノードがもつ値より大きな値が入る

入れ子構造設計の大きな長所は非葉ノードを悪女すると削除されたノードの子孫が削除されたノードの親の直接の子であると自動的に見なされる点。
しかし入れ子構造では直近お親の取得などの隣接リストでは簡単に実行できるクエリの一部が複雑になる。
個々のノードの操作ではなく、サブツリーに対する迅速かつ容易なクエリ実行が重要な場合に有効。
ノードの挿入や移動は関連するノードの左右値の再計算が必要になるので複雑になる。

### 閉包テーブルモデル(Closure Table)

階層構造のシンプルかつエレガントな格納方法。
直接の親子関係だけでなく、ツリー全体のパスを格納する。

シンプルなcommentsテーブルに加えてtree_pathsテーブルを新たに定義する。
tree_pathsでは先祖/子孫関係を共有するノードの組み合わせを格納する。
ツリー上の離れた位置にあるノードも含めたすべてのノードが対象になる。

ただ、直近の子や親へのクエリがやや複雑になる。
しかしpath_lengthカラムを追加すれば簡単に使用できるようになる。

## ③IDリクワイアド(とりあえずID)

***主キー(primary key)*** の理解。
テーブルのすべての行が一意であることを保証するものであり、
個々の行を扱い、行の重複を避けるために必要な論理的メカニズム、
主キーは ***外部キー(foreign key)*** から参照されることで、テーブルの関連付けを行う。
ある対象領域をモデル化したテーブルに、その領域で意味を持たない人工的な値を格納するには
新たな列を追加する。
この列を主キーとして使用することで、他の属性列には重複を許可しながら、行を一意のものとして扱えるようにする。
***擬似キー(pseudo key)*** や ****代理キー(surrogate key)*** と呼ばれる。

主キーの役割

- 行の重複を避けたい
- クエリで個別の行を参照したい
- 外部キー参照をサポートしたい

すべてのテーブルに「id」列を用いるというアンチパターン

### デメリット

- 冗長なキーが作成される...同じテーブルにある別の列が ***「自然な」主キー(ナチュラルキー)*** として使えそうな時や、UNIQUE制約を付与できそうなとき冗長になる
- 重複行を許可してしまう...***複合キー(compound key)***は複数の列で構成されたーキー(BugsProductsテーブルの主キーはbug_idとproduct_idの組み合わせといった場合)が一意であることを保証しなければならない。しかしid列を主キーとすると組み合わせが一意であることを保証しなくなる
- キーの意味がわかりにくくなる...idという列名は明確な意味をもたない。
- USINGを使用する...USINGを使用する際は従属テーブル側の外部キー列には参照する主キーと同じ名前が使用できず、冗長なON構文を使用することになる。
- 複合キーは使いにくい...単純化できるにも関わらず対象とすべき現実世界の物体を正確に表していない

### アンチパターンを用いてもいい場合

一部のオブジェクトリレーショナルマッピング(ORM)フレームワークは開発をシンプルにするため
***設定より規約(convention over configuration)*** の原則に従ってる。
ただ、この規約を上書きすることもできるので状況に応じて適切に調整すべき。


## ④キーレスエントリ(外部キー嫌い)

リレーショナルデータベースの設計において、各テーブルの設計と同じくらい重要なのが、
テーブル間の関連(リレーションシップ)の設計。
***参照整合性*** は、適切なデータベース設計と運用において極めて重要。

参照整合性を使用しない(外部キーを使用しない)主な理由

- データの更新が参照整合性制約と衝突
- 設計の柔軟性が極めて高いので参照整合性をサポートでkない
- 外部キーのために作成するインデックスが、パフォーマンスに影響する
- 外部キーをサポートしないDB製品を使用している
- 外部キーを宣言する構文を調べなければならない

外部キー制約を使用しないというアンチパターン。

### デメリット

- 完璧なコードを前提...ミスをしないようするということが前提になる
- ミスを調べなければならない...数百ものチェックを毎日、あるいは1日に複数回実行しなくてはならないことにもなり得る
- キャッチ=22なUPDATE...親と子を同時に変更しなければならない場合が生じる。2つの異なる更新処理を同時に実行することは不可能。これを避けたい場合にも外部キーを用いて解決できる。(カスケード更新: 外部キー制約にON UPDATE,ON DELETE句を宣言)

### アンチパターンを用いてもいい場合

外部キー制約をサポートしていないDB製品を使用する場合。(MySQLのMyISAMストレージエンジンやバージョン3.6.19未満のSQLite)

## ⑤EAV(エンティティ・アトリビュート・バリュー)

日付でグルーピングするような場合、以下の前提がある。

- 値が同じ列に格納されている
- 値を比較できる

日付のフォーマットが異なったり、違うカラムにデータが保存されていると簡単には比較できない。

汎用的な属性テーブルを使用するというアンチパターン(ex: attr_name, attr_value)

### デメリット

- 必須属性を設定できない
- データ型を使えない
- 参照整合性を強制できない
- 属性名を補わなければならない
- 行を再構築しなければならない

### アンチパターンを用いてもいい場合

リレーショナルデータベースの多くの長所が失われてしまうため、EAVを正当化する理由は少ない。
***非リレーショナル*** なデータ管理が必要なら非リレーショナルな技術を使用するべき。

- Berkeley DB...key/value式のDBLibrary
- Cassandra...Facebookで開発された列指向の分散DB
- CouchDB...ドキュメント指向DB。分散型のkey/valueを格納し値をJSON形式でencode
- Hadoop/HBase...GoogleのMapReduceアルゴリズムを実装したオープンソース分散DB。
- MongoDB...CouchDBと類似したドキュメント指向のDB
- Redis...ドキュメント指向のインメモリーDB
- Tokyo Cabinet...key/value型のデータストアでPOSIX DBM, GNU GDBM,Berkeley DBなどに対応している。

***EAVの解決策***

- シングルテーブル継承
- 具象テーブル継承
- クラステーブル継承


## ⑥ポチモーフィック関連

2つの類似したテーブルが存在する場合

二重目的の外部キーを使用するというアンチパターン

- ポリモーフィック関連(polymorphic associations)
- プロミスキャス・アソシエーション(promiscuous association: 無差別な関連)

複数のテーブルを参照できる。

### デメリット


- 参照整合性制約を定義できない...この場合、 `issue_id` のような外部キーに加えて `issue_type` といった関連付けのためのカラムが必要になる。この場合 `issue_id` のための外部キー宣言ができない。外部キーではテーブルを1つのみ指定しなければならない。
- 両方の親テーブルを外部結合したクエリの実行が必要になることも
- 非オブジェクト指向

### アンチパターンを用いてもいい場合

なるべくポリモーフィック関連の使用は避けて外部キー制約を用いて参照整合性を保証するようにする。
ポリモーフィック関連を使用するとメタデータではなくアプリケーションコードに過度に依存してしまう。
HibernateなどのORMフレームワークを用いる場合はこのアンチパターンを使わざるを得ないときがある。
ポリモーフィック関連固有のアプリケーションロジックをカプセル化して参照整合性を維持することでポリモーフィック関連の採用によるリスクを低減させている。

## ⑦マルチカラムアトリビュート

ジェイウォークと同じ1つのテーブルに属するべきだと思える属性に複数の値がある場合、それをどう格納するかという問題。
バグデータベースに、バグを分類するためのタグ付け機能を追加したい場合。

複数の列を定義するというアンチパターン。
服数値をカンマ区切りで1列に格納するのではなく、列を追加するパターン。

### デメリット

- 値の検索...手間がかかる
- 値の追加と削除...複雑なSQLが必要
- 一意性の保証...複数の列に同じ値が格納される可能性
- 増加する値の処理...タグ数の最大値に応じて拡張が必要

### アンチパターンを用いてもいい場合

属性の選択肢を限定できる場合。

## ⑧メタデータトリブル(メタデータ大増殖)

テーブルや列をコピーするというアンチパターン

- 行数の多いテーブルを、複数のテーブルに分割する(あるテーブルの属性の区別しやすいデータ値に基づいてテーブルを命名する)
- 列を複数列に分割する(別の属性の区別しやすい値に基づいて列を命名する)

### デメリット

- テーブルの増殖...新しいデータのために新たなメタデータオブジェクトを作成しなければならない場合がある
- データの整合性の管理...CHECK制約の値の修正し忘れを防ぐ必要性
- データの同期
- 一意性の保証
- テーブルをまたいだクエリ実行
- メタデータの同期
- 参照整合性の管理
- メタデータトリブル列の特定

### アンチパターンを用いてもいい場合

過去データを最新のデータから分離するようなアーカイブ
過去データに対してクエリを実行する必要性は大幅に低下する

***解決策***

パーティショニングと正規化を行う

- 水平パーティショニング
- シャーディング
- 垂直パーティショニング