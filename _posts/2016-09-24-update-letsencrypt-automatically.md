---
layout: post
title:  "How to automatically renew certificates"
date:   2016-09-24 13:09:43 +0900
categories: Development
---

When I was looking at the mail today, something like the one below arrived.

```
Hello,

Your certificate (or certificates) for the names listed below will expire in 1 days (on 08 Jul 16 02:04 +0000). Please make sure to renew your certificate before then, or visitors to your website will encounter errors.

yukihirai0505.com

For any questions or support, please visit https://community.letsencrypt.org/. Unfortunately, we can't provide support by email.

If you are receiving this email in error, unsubscribe at http://mandrillapp.com/track/unsub.php?u=30850198&id=6bf0b70cda22463db1370e46485dd110.V0K2NHoiq8zwxBwbY4n1U1LGVPo%3D&r=https%3A%2F%2Fmandrillapp.com%2Funsub%3Fmd_email%3Dyukihirai0505%2540gmail.com. (HTTP link, we know. We're working on it!)

Regards,
The Let's Encrypt Team
```

Looking at the contents,
please renew it because SSL certificate set in LetsEncrypt will expire.
It is troublesome to do it manually every time
so I decided to use cron to update it automatically.

## Creating a shell script

First of all,
I created a shell script to update LetsEncrypt in the server's home directory.

`renew_letsencrypt.sh`

```
#!/bin/sh
cd letsencrypt/
git pull
sudo service nginx stop
./letsencrypt-auto renew --force-renew
sudo service nginx start
```

## Setting crontab

I added a new job to crontab,
And then I added a job to run the above shell script at the beginning of the month to `crontab - e`.

```
0 0 1 * * sh ~/renew_letsencrypt.sh
```

## Execute the shell script with sudo

This shell script contains `sudo` .
I set sudo permission so that it can be executed without entering a password.

```
sudo visudo
```

After entering the password,
I added a following sentence at the end of the file

```
%{USERNAME}   ALL=(ALL)       NOPASSWD: ALL
```

I have successfully set up LetsEncrypt's automatic update.
