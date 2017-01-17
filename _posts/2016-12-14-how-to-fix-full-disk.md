---
layout: post
title:  "When you want to recover quickly when the disk capacity is full"
date:   2016-12-14 12:00:00 +0900
categories: Development
---

In this case,
when the disk capacity becomes full and the system is down, record about immediate actions.

## When the disk capacity is full and the system goes down

When the disk capacity becomes full, the system may go down.
If this is a production environment it is highly urgent so I'd like to recover quickly.

This time I will describe the response of immediate recovery
rather than solving the fundamental problem.

## Check which dir is full

First of all, we should check which directory is using capacity with `du -sh` command.

## Check the files in the target directory

Check the file in the target directory and check the contents if there is error.log etc.
If there is a lot of logs expired with some error,
restart the system once,
save the target log to another server and delete it from the target server.
And then, lighten the disk capacity as much as possible.

## Precautions for system down

- Install log rotation
- Configure disk space alerts

For AWS, you can set alert of disk capacity in CloudWatch with custom setting.
The following article is helpful.

- [Monitoring Memory and Disk Metrics for Amazon EC2 Linux Instances](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/mon-scripts.html)

If you run for the first time,
it may be that some perl modules are missing and it may not work,
so let's check the error log for proper installation.

Also, the `install Crypt :: SSLeay` test may not work,
but it seems that you can set TEST to NO. ←
I do not think so well. lol

[How to Force Install a Perl Module using CPAN](http://www.thegeekstuff.com/2013/06/cpan-force-install-perl-module)

qt: `fforce install Net::SSLeay`