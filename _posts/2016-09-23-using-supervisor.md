---
layout: post
title:  "Supervisorでプロセス管理"
date:   2016-09-23 13:09:43 +0900
categories: Development
---

今まで個人でPlayframeworkを使用して作成したアプリをデプロイする際にfabricからデプロイしていて、
`RUNNING_PID`ファイルを直接指定してプロセスkillして~みたいな内容だったので
python製のプロセス管理ツールである[Supervisor](http://supervisord.org/)を使用していきます。

最終的にはfabricの内容を下記のような感じにします。

**before**

{% highlight python %}
if run('ps -ef | grep "%(root)s/target/universal/stage" | grep -v grep | wc -l' % {"root": env.project_name}) == "1":
  with cd(env.server_dir):
    run("kill `cat %(root)s/target/universal/stage/RUNNING_PID`" % {"root": env.server_dir})
{% endhighlight %}

**after**

{% highlight python %}
run("supervisorctl stop [プロセス名]")
{% endhighlight %}

## supervisorのインストール

まずは下記URLを参考にインストールを進めていきます。
リモートサーバーに`supervisor`をインストールしていきます。

http://supervisord.org/installing.html

```
yum install python-setuptools
easy_install pip
pip install supervisor
```

## supervisord.confの設定

次に設定ファイルを作成します。

`supervisor`をインストールすると
`echo_supervisord_conf`というコマンドが使用できるようになるので、下記のように設定ファイルを作成します。

```
echo_supervisord_conf > /etc/supervisord.conf
```

権限によっては指定できない場合もあるので、
作成場所は各々で。

ファイルに今回追加したい処理を記述しておきます。
自分の場合だと今回はPlayframworkを起動するcommandを下記に記述しておきます。

```
[program:sample-daemon]
process_name=%(program_name)s
directory=[hogehoge]
command=/bin/bash -c "/path/to/target/universal/stage/bin/app"
autostart=true
autorestart=true
user=hogehoge
redirect_stderr=true
stdout_logfile=/dev/null
```

上記の設定を細かく見ていくと

- `program:sample-daemon`
    - プロセスの名前(今回はsample-daemonとしている)
- `command`
    - 実行する処理の記述
- `autostart=true`
    - supervisorが起動したら自動的に起動する
- `autoretart=true`
    - 落ちても自動的に再起動する
- `user=hogehoge`
    - 処理を実行するユーザーを記述する
- `redirect_stderr=true`
    - エラー出力を標準出力にリダイレクトする
- `stdout_logfile=/dev/null`
    - 標準出力を捨てる(ログをとりたい場合はファイルパスを指定)

次に

```
supervisord -c supervisord.conf
```

としてあげます。

## supervisorctlの実行

これで下記コマンドで処理を実行することができます。

```
supervisorctl start sample-daemon
```

処理をストップするときは

```
supervisorctl stop sample-daemon
```

ここまでくれば`fabric`で

{% highlight python %}
if run('ps -ef | grep "%(root)s/target/universal/stage" | grep -v grep | wc -l' % {"root": env.project_name}) == "1":
  with cd(env.server_dir):
    run("kill `cat %(root)s/target/universal/stage/RUNNING_PID`" % {"root": env.server_dir})
{% endhighlight %}

みたいに書いていたところを

```py
run("supervisorctl stop sample-daemon")
```

もちろんアプリを走らせる処理も

```py
run("supervisorctl start sample-daemon")
```

でオッケーです。

とシンプルに記述することができます。
