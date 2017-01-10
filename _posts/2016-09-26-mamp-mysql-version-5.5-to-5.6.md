---
layout: post
title:  "How to upgrade MAMP's MySQL version from 5.5 to 5.6"
date:   2016-09-26 13:09:43 +0900
categories: Development
---

Today, since there was an error because the version of MySQL was different on PC using MAMP, I had the opportunity to change from 5.5 to 5.6.
I would like to write a simple flow here instead of memos.

## Using a handy script

When I searched "MAMP MySQL 5.6" on google,
someone prepared a script that looks good.

The link is here

[https://gist.github.com/tobi-pb/b9426db51f262d88515c](https://gist.github.com/tobi-pb/b9426db51f262d88515c)

Since there are quite a lot of discussions in the comment section, I think whether it will be optimized according to each develop environment from time to time.

[https://gist.github.com/tobi-pb/b9426db51f262d88515c](https://gist.github.com/tobi-pb/b9426db51f262d88515c)

Before downloading this script and running it, let's install the command you need, such as wget.

[http://d.hatena.ne.jp/replication/20150214/1423879393](http://d.hatena.ne.jp/replication/20150214/1423879393)

And then, running this script

```
sh migrate.sh
```

## If you encounter an error...

Perhaps the above shell script may end with an error depending on the develop environment.
But on 05/13/2016, this script shows log.
We can find out where the error is occurring.

- Each phase
    - "stopping mamp"
    - "creating backup"
    - "copy bin"
    - "copy share"
    - "fixing access (workaround)"
    - "starting mamp"
    - "migrate to new version"

## Summary

It is very helpful.
We can work efficiency because of such as script.
