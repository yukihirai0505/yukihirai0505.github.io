---
layout: post
title:  "A Process Control System — Supervisor"
date:   2016-09-23 13:09:43 +0900
categories: Development
---

I used fabric when deploying applications created with Playframework so far.
The content was that it first kills the process by directly specifying the `RUNNING_PID` file to stop the application.
This time, I will use [supervisor] (http://supervisord.org/) which is a process control tool made by python.

Ultimately, I will make the contents of the fabric feel like the following.

**before**

{% highlight python %}
if run('ps -ef | grep "%(root)s/target/universal/stage" | grep -v grep | wc -l' % {"root": env.project_name}) == "1":
  with cd(env.server_dir):
    run("kill `cat %(root)s/target/universal/stage/RUNNING_PID`" % {"root": env.server_dir})
{% endhighlight %}

**after**

{% highlight python %}
run("supervisorctl stop [process name]")
{% endhighlight %}

## Installing supervisor

First you proceed with installation referring to the following URL.
you should install `supervisor` on the remote server.

http://supervisord.org/installing.html

    yum install python-setuptools
    easy_install pip
    pip install supervisor

## Setting supervisord.conf

Next, you create a configuration file for supervisor.

After installing `supervisor`
you can use `echo_supervisord_conf` command.

    echo_supervisord_conf > /etc/supervisord.conf

Depending on the permission you may not be able to specify,
so you choose the place of creation.

And then, you describe the processing you want to add this time to the file.
In case of myself, here is the command to start Playframwork in this time.

    [program:sample-daemon]
    process_name=%(program_name)s
    directory=[hogehoge]
    command=/bin/bash -c "/path/to/target/universal/stage/bin/app"
    autostart=true
    autorestart=true
    user=hogehoge
    redirect_stderr=true
    stdout_logfile=/dev/null

I will explain the above setting in detail.

- `program:sample-daemon`
    - The name of the process
- `command`
    - Description of the processing to be executed
- `autostart=true`
    - Automatically starts when supervisor starts up
- `autoretart=true`
    - Automatically restart even if it stops
- `user=hogehoge`
    - Describe the user executing the process
- `redirect_stderr=true`
    - Redirect error output to standard output
- `stdout_logfile=/dev/null`
    - Discard the standard output (specify the file path if you want to log)

Next, to start supervisor
you should execute following command.

    supervisord -c supervisord.conf

## Starting supervisorctl

Now you can execute the processing with the following command.

    supervisorctl start sample-daemon

to stop this process

    supervisorctl stop sample-daemon

After all, this `fabric` file 

{% highlight python %}
if run('ps -ef | grep "%(root)s/target/universal/stage" | grep -v grep | wc -l' % {"root": env.project_name}) == "1":
  with cd(env.server_dir):
    run("kill `cat %(root)s/target/universal/stage/RUNNING_PID`" % {"root": env.server_dir})
{% endhighlight %}

to like this

    run("supervisorctl stop sample-daemon")

    run("supervisorctl start sample-daemon")

It can be described simply.
