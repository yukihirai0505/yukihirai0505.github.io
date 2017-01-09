---
layout: post
title:  "LetsEncryptを自動更新するスクリプトの作成"
date:   2016-09-24 13:09:43 +0900
categories: Development
---

今回メールを見ていたら下記のようなものが届きました。

```
Hello,

Your certificate (or certificates) for the names listed below will expire in 1 days (on 08 Jul 16 02:04 +0000). Please make sure to renew your certificate before then, or visitors to your website will encounter errors.

yukihirai0505.com

For any questions or support, please visit https://community.letsencrypt.org/. Unfortunately, we can't provide support by email.

If you are receiving this email in error, unsubscribe at http://mandrillapp.com/track/unsub.php?u=30850198&id=6bf0b70cda22463db1370e46485dd110.V0K2NHoiq8zwxBwbY4n1U1LGVPo%3D&r=https%3A%2F%2Fmandrillapp.com%2Funsub%3Fmd_email%3Dyukihirai0505%2540gmail.com. (HTTP link, we know. We're working on it!)

Regards,
The Let's Encrypt Team
```

内容を見てみると、LetsEncryptで設定したSSLの証明書がきれるから更新してね。とのこととでした。
毎回手作業でやるのは面倒なのでcronを使用して、自動更新するようにしました。

## シェルスクリプトの作成

まずは、LetsEncryptを更新するシェルスクリプトをサーバーのホームディレクトリに作成しました。

`renew_letsencrypt.sh`を作成する。

```
#!/bin/sh
cd letsencrypt/
git pull
sudo service nginx stop
./letsencrypt-auto renew
sudo service nginx start
```

## crontabの設定

次にcrontabに新しいジョブを追加します。

`crontab -e`で毎月月初の深夜に上記のシェルスクリプトを実行するジョブを追加します。


```
0 0 1 * * sh ~/renew_letsencrypt.sh
```

ただ、期限が切れる30日以内じゃないと証明書は更新されないので
まあどのタイミングにするかはお好みでどうぞ。

```
renew only renews certificates when they are less than 30 days from expiration, allowing you to run it in something like cron and only renew the cert when necessary.
```

## シェルスクリプトでsudoの実行

最後にシェルスクリプトで`sudo`を実行しているので、
パスワード入力を受け付けなくてもいいようにsudo権限を与えるように設定します。

```
sudo visudo
```

でパスワードを入力して
ファイルの末尾に

```
%実行ユーザー名   ALL=(ALL)       NOPASSWD: ALL
```

を追記してあげれば大丈夫です。

以上で、LetsEncryptの自動更新の設定ができたかと思います。

まだ実際に動いてないのでちゃんと更新するかわかりませんが 笑
めちゃめちゃお手軽ですね。

ちなみに、今回はこのスクリプトを実行してちゃんと証明書更新されているみたいでした。
