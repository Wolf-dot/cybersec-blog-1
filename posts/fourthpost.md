---
title: This is my fourth post.
description: Draft
date: 2021-06-31
scheduled: 2021-06-31
layout: layouts/post.njk
tags:
    - second-tag
    - posts
---

This is optional since this is just three containers talking, but we could use https instead of http.
SSL is very important as simple http provides no encryption, that's why we had to use `-k` flag to ignore the warnigns.

To use SSL we need a certificate which is used during a handshake and confirms our identity.
You can go to websites which provide paid or free certificates, but we're going to use a self signed certificate.
A self-signed certificate still would show a warning on your browser, but we'd graduate to https.

I'll use openssl to generate self-signed certificates for both the attacker and reverse-proxy.
reverse-proxy: 

``` bash
apk add openssl
```

attacker: 

``` bash 
apt install openssl
```

In reverse-proxy go to `/etc/ssl/` and create a directory named 'private': 

``` bash
mkdir private
```

This directory should be already present in attacker.
Now generate the public and private key's with the following command:

``` bash
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/nginx-selfsigned.key -out /etc/ssl/certs/nginx-selfsigned.crt
```
