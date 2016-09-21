---
layout: post
title:  "さくらのVPSで無料SSL設定(Nginx)"
date:   2016-09-30 13:09:43 +0900
categories: Development
---

今回は自分のサーバーに無料SSLを導入してみました。

## ますはオレオレで練習

まずはオレオレの証明書で導入の練習してみました。

[http://qiita.com/duke-gonorego/items/afbbcd7044d3da178723](http://qiita.com/duke-gonorego/items/afbbcd7044d3da178723)

これで指定のURLにアクセスしたら良い感じにオレオレでした。

## LetsEncryptで無料SSL設定

これで導入のイメージはつかめたので、
次はオープンソースの無料証明書を設定してみます。

こちら非常にお手軽でした。
導入手順は下記の記事を参考に進めました。

[http://tech.mof-mof.co.jp/blog/letsencrypt.html](http://tech.mof-mof.co.jp/blog/letsencrypt.html)

これでアクセスすれば良い感じにhttpsでアクセスできます。

## LetsEncryptの注意点

ただ、無料証明書だとAPIで使用で使用を制限しているものがあったり、
更新回数が頻繁だったりするので注意が必要です。

> Let's Encrypt CA が発行する SSL/TLS サーバ証明書の有効期間は、短期間（90日間）です。
> 少なくとも、3か月に一回は、証明書の更新を行なう必要があります。


[https://letsencrypt.jp/docs/using.html](https://letsencrypt.jp/docs/using.html)


また、短期間に更新しすぎると制限がかかってしまうこともあるそうです。

```
Rate limit on registrations per IP is currently 10 per 3 hours
Rate limit on certificates per Domain is currently 5 per 7 days
```

[https://community.letsencrypt.org/t/public-beta-rate-limits/4772](https://community.letsencrypt.org/t/public-beta-rate-limits/4772)


ただ、無料でSSLが設定できて気持ちがいい(笑)ので手軽に導入したい人にはオススメです^^
