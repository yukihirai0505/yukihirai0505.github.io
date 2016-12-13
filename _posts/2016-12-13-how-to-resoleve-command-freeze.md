---
layout: post
title:  "コマンド(yumなど)を実行したときにフリーズするときの対処法"
date:   2016-12-13 12:00:00 +0900
categories: Development
---

今回は

- `yum install perl-Switch perl-Sys-Syslog perl-LWP-Protocol-https`
- `yum search hogehoge`
- `yum clean all`

などのようにコマンドを実行しても何も表示されずにフリーズしてしまう場合の対処法をメモしておきます。

## ログの確認
 
まずはログを確認してみます。

`/var/log/yum.log` の確認
特にエラーはなし。

## 再現

今回のフリーズするという事象が再現するかコマンドを実行してみる。

`yum install perl-Switch perl-Sys-Syslog perl-LWP-Protocol-https`
確かにうんともすんとも言わない

→ (仮定)yumコマンドでDLすら始まらないので、ロックがかかってるんじゃないか？

## プロセスの確認

`ps -ax | grep yum`

yumプロセスが複数ある。

```
18787 ?        S      0:00 /usr/bin/python /bin/yum install perl-Switch perl-DateTime perl-Sys-Syslog perl-LWP-Protocol-https
27461 ?        S      0:00 /usr/bin/python /bin/yum search perl-Switch
27597 ?        S      0:00 /usr/bin/python /bin/yum clean all
28032 pts/10   S+     0:00 /usr/bin/python /bin/yum install perl-Switch perl-Sys-Syslog perl-LWP-Protocol-https
```

→ ロックがかかって後発が動いてない

## プロセスのkill

試しにプロセスNoが若い順からkillして止める。
再度yum実行
→ 通る

## 原因

おそらく

- 初回のyum実行時に落ちた
- 容量の問題で正しくDL・インストールできなかった

のではないかと思われます。