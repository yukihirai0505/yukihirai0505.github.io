---
layout: post
title:  "How to deal with a problem that freezing when executing command (yum, etc.)"
date:   2016-12-13 12:00:00 +0900
categories: Development
---

Today, I will explain how to deal with a problem when executing command like following.

- `yum install perl-Switch perl-Sys-Syslog perl-LWP-Protocol-https`
- `yum search hogehoge`
- `yum clean all`

## Check the log

Let's first check the log.

`/var/log/yum.log`

## Test whether to reproduce

Let's try to execute the command to reproduce the event of freezing.

`yum install perl-Switch perl-Sys-Syslog perl-LWP-Protocol-https`

If it freezes, 

→ (Assumption) Since the DL does not begin with the yum command, is not it locked?

## Check the process

`ps -ax | grep yum`

There are multiple yum processes.

```
18787 ?        S      0:00 /usr/bin/python /bin/yum install perl-Switch perl-DateTime perl-Sys-Syslog perl-LWP-Protocol-https
27461 ?        S      0:00 /usr/bin/python /bin/yum search perl-Switch
27597 ?        S      0:00 /usr/bin/python /bin/yum clean all
28032 pts/10   S+     0:00 /usr/bin/python /bin/yum install perl-Switch perl-Sys-Syslog perl-LWP-Protocol-https
```

→ Lock is applied and the sequelae do not move

## Kill the process

Try to kill process no. From the youngest to stop.
=> Run yum again
=> It may go through

## Check the cause

Probably

- Falls during initial yum execution
- DL can not install properly due to capacity problem

It seems to have been frozen for the reasons mentioned above.

This problem solving process seems to be applicable when other problems occur.