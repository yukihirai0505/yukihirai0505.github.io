---
layout: post
title:  "Nginx with Let's Encrypt on CentOS"
date:   2016-09-30 13:09:43 +0900
categories: Development
---

This time I tried introducing free SSL to my server.

## Free SSL settings with LetsEncrypt

I set up an open source free certificate.

Here it was very easy.
I proceeded with reference to the following articles for introduction procedures.

[How To Secure Nginx with Let's Encrypt on CentOS 7](https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-centos-7)

After that,
I could access it with https.

## Remarks of LetsEncrypt

However, if it is a free certificate,
there are things that restrict the use by using the API,
and care should be taken because the number of updates is frequent.

The validity period of SSL / TLS server certificate issued by Let's Encrypt CA is short term (90 days).
At least, we need to renew the certificate once every three months.

[https://letsencrypt.org/2015/11/09/why-90-days.html](https://letsencrypt.org/2015/11/09/why-90-days.html)


Also, there seems to be a limitation if updating too much in a short period of time.

```
Rate limit on registrations per IP is currently 10 per 3 hours
Rate limit on certificates per Domain is currently 5 per 7 days
```

[https://community.letsencrypt.org/t/public-beta-rate-limits/4772](https://community.letsencrypt.org/t/public-beta-rate-limits/4772)


However, I recommend LetsEncrypt for people who want to introduce ssl for free
because it is easy to set SSL :)
