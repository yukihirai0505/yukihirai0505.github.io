---
layout: post
title:  "How to register a library made by Scala to the Maven Central Repository"
date:   2016-11-13 12:00:00 +0900
categories: Development
---

Today, I'd like to explain how to register a library made by Scala to the Maven Central Repository.
I use sbt (ver 0.13).

## Introduction

I created Scala API Wrapper for Instagram named sInstagram.
Since other Scala API Wrapper content was old, I made it myself.

How to register a library to Maven Central Repository is like the following procedure.

- Create a Scala library and push to Github
- Create an account with Sonatype JIRA
- Application for project at Sonatype JIRA
- Create and register keys with the GPG
- Fix build.sbt
- Deploy to Sonatype OSSRH
- Released on Sonatype OSSRH

The following link is very helpful.


- [GPG](https://gpgtools.org/)
- [Sonatype](https://issues.sonatype.org/secure/Dashboard.jspa)
- [Publishing](http://www.scala-sbt.org/0.13/docs/Publishing.html)

## Using build.sbt to register a library to Maven Central Repository  

The following URL is helpful for the deployment work in build.sbt.

- [Deploying to Sonatype](http://www.scala-sbt.org/0.13.1/docs/Community/Using-Sonatype.html)

First add the necessary libraries to Scala's project.

Let's edit this file `./project/plugins.sbt` 

```
addSbtPlugin("com.jsuereth" % "sbt-pgp" % "1.0.0")
addSbtPlugin("org.xerial.sbt" % "sbt-sonatype" % "0.2.1")
```

And then edit this file `~/.sbt/0.13/sonatype.sbt` 

```
credentials += Credentials("Sonatype Nexus Repository Manager",
    "oss.sonatype.org",
    "username",
    "password")
```

After that, edit this file `./build.sbt` 

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

If you set up so far and have registered GPG key, then
you should run `sbt` command.

Let's not forget to register keys created with GPG on the server.

You can deploy with `publishSigned` on sbt console.

I write in here links etc. that helped me with errors encountered.
Because I could not successfully register the key of GPG,
I could not close it with an error when closing the project that I deployed.
Also, if the package name specified in `orgranization` is different from the GroupId of the project you are applying to, you may get an error on release.

- [No public key: Key with id: (XXXXX) was not able to be located (oss.sonatype.org)](http://stackoverflow.com/questions/19462617/no-public-key-key-with-id-xxxxx-was-not-able-to-be-located-oss-sonatype-org)