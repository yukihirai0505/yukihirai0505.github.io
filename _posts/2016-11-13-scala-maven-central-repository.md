---
layout: post
title:  "Scalaで作成したライブラリをMavan Central Repositoryに登録する手順"
date:   2016-11-13 12:00:00 +0900
categories: Development
---

今回は自分で作成したLibraryをMaven Central Repositoryに登録したので、
その手順についてまとめておきたいと思います。

今回はsbt(ver 0.13)を使用して設定などを記述していきます。

## はじめに

今回はScalaでsInstsagramというInstagramのAPI Wrapperを作成致しました。
他のScala製のAPI Wrapperの内容が古かったりしたので、
作ってしまいました。

Maven Central Repositoryへの登録手順は簡単に下記のようになります。

- Scalaライブラリの作成してGithubにpush
- Sonatype JIRA でアカウントの作成
- Sonatype JIRA でプロジェクトの申請
- GPGで鍵の作成とサーバーへの登録
- build.sbtを修正
- Sonatype OSSRHにデプロイ
- Sonatype OSSRHでリリース

ほとんど下記のURLで説明されてますが、build.sbtの部分だけ異なるのでここでは
そこに絞って書いておいます。

=> [GitHub で公開したソースコードを Maven Central Repository に登録する手順](https://blog.tagbangers.co.jp/2015/02/27/to-register-the-source-code-that-was-published-in-github-to-maven-central-repository)

- [GPG](https://gpgtools.org/)
- [Sonatype](https://issues.sonatype.org/secure/Dashboard.jspa)

## build.sbtでMavan Central Repositoryへの登録

build.sbtでのデプロイ作業は下記のURLが参考になります。

- [Deploying to Sonatype](http://www.scala-sbt.org/0.13.1/docs/Community/Using-Sonatype.html)

まず必要なライブラリをScalaのプロジェクトに追加します。

`./project/plugins.sbt` ファイルに下記ライブラリを追加します。

```
addSbtPlugin("com.jsuereth" % "sbt-pgp" % "1.0.0")
addSbtPlugin("org.xerial.sbt" % "sbt-sonatype" % "0.2.1")
```

次に `~/.sbt/0.13/sonatype.sbt` ファイルに

```
credentials += Credentials("Sonatype Nexus Repository Manager",
    "oss.sonatype.org",
    "username",
    "password")
```

ファイルを作成します。

あとは

`./build.sbt` ファイルを編集してあげます。

```
import SonatypeKeys._

sonatypeSettings

name := "appName"

version := "librari version"

scalaVersion := "Scala version"

publishMavenStyle := true

publishTo := {
  val nexus = "https://oss.sonatype.org/"
  if (isSnapshot.value)
    Some("snapshots" at nexus + "content/repositories/snapshots")
  else
    Some("releases" at nexus + "service/local/staging/deploy/maven2")
}

publishArtifact in Test := false

pomIncludeRepository := { _ => false }

organization := "package name"

organizationHomepage := Some(url("Your Home Page URL"))

description := "library description"

pomExtra :=
  <url>https://github.com/yukihirai0505/sInstagram</url>
  <licenses>
    <license>
      <name>LICENSE Name</name>
      <url>LICENSE URL</url>
      <distribution>repo</distribution>
    </license>
  </licenses>
  <scm>
    <url>repositroy url</url>
    <connection>scm:git:repository url</connection>
  </scm>
  <developers>
    <developer>
      <id>sonatype account name</id>
      <name>Your name</name>
      <url>your home page</url>
    </developer>
  </developers>
```

ここまで設定してGPGの鍵の登録も済んでいればあとは

`sbt` コマンドを入力して

GPGで作成した鍵をサーバーに登録しておくのは忘れないようにしておきましょう。

`publishSigned` コマンドを実行すればデプロイ完了します。

自分が出くわしたエラーで役立ったリンクなどをここに書いておきます。
GPGの鍵がうまく登録できなかったためにデプロイしたプロジェクトのcloseの際にエラーがでてcloseできませんでした。
また、 `orgranization` で指定しているpackage nameと申請しているプロジェクトのGroupIdが異なっている場合releaseの際にエラーになることがあります。

- [No public key: Key with id: (XXXXX) was not able to be located (oss.sonatype.org)](http://stackoverflow.com/questions/19462617/no-public-key-key-with-id-xxxxx-was-not-able-to-be-located-oss-sonatype-org)