---
layout: post
title:  "Happy Deploy Life with Fabric"
date:   2016-10-03 13:09:43 +0900
categories: Development
---

## What is Fabric

Fabric is

- A tool to execute arbitrary ***Python function*** via the command line.
- A library of subroutines (built on low-level libraries) ***easily via SSH*** and ***execute Python-like*** shell command.

Fabric can automate the deployment work.
You do not have to manually access and work on the server each time.
For example, I wrote a file that created a method called yum_update as shown below.

{% gist yukihirai0505/cd46406c76e578c6d773 %}

And then, you can use this file like a following command.

{% gist yukihirai0505/f7e4057692bf980f0c61 %}

## Setting for Fabric

In the above command,
We can run the command by specifying the user name, secret key of ssh, host name.
However, they can also be written on a file.
This time, what I made for my server is as follows.

{% gist yukihirai0505/5964130f78d6fc09414e %}


Here is the point.

    env.hosts = ['host name']
    env.user = 'user name'
    env.key_filename = 'key file path'

And then, you can use a following command.
It is simple.

    fab [execute method]

To explain this file a little.
You can see that lcd() and local() are used for local processing.
We are sending files on the remote server with put().
To run the command on the remote server, use run().

## Deployment Automation happiness

We can leave the deployment process with this Fabric.

    fab deploy

This command is just simple.
It is very convenient.
Everyone can spend a happy deployment life by using Fabric :)