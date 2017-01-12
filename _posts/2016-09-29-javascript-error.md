---
layout: post
title:  "Uncaught SyntaxError: Unexpected token ILLEGAL ・・・ on Javascript Console"
date:   2016-09-29 13:09:43 +0900
categories: Development
---

Since I was suffering from errors,
So I write it on this blog.

I thought that I wanted to make changes to a certain Javascript file,
and when I edited it a bit, I got a mysterious error.

What kind . .

By the way, it comes only for the number of characters I enter.

Mystery error.

    Uncaught SyntaxError: Unexpected token ILLEGAL

However, it is not reproduced by other developers.

Replacing Nginx's configuration with Apache,
I rewrote the httpd.conf file like following.

    EnableSendfile off

Then I restarted Apache.
Safely error has been resolved.

It was a very bad error.
