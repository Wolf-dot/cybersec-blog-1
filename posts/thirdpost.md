---
title: Creating reverse-proxy.
description: Draft
date: 2021-06-30
scheduled: 2021-06-30
layout: layouts/post.njk
tags:
    - posts
---

You can make a fake website, create a download link, whatever. The attack possibilites are almost endless.
But your attacking server is left open! If you were blacklisted you'd have to spin up and configure a whole new server.
But there is a way!

A reverse proxy is a server that sits in front of web servers and forwards client (e.g. web browser) requests to those web servers.
Which means that the attacker will be hidden behind the proxy, and if the site is blacklisted - it's no problem for a reverse-proxy, redeploying it takes minutes.

So, let's go into Reverse-Proxy and navigate to `/etc/nginx/conf.d` and open default.conf.
Let's add a new location which will pass the requests to the IP address of our Attacker.

``` bash
    location /merlin {
        proxy_pass http://172.17.0.3/;
        }
```

![console prtsc](/img/remote/edit-reverse-proxy-server.png)

Reload nginx: `nginx -s reload`.

Now let's go to Victim and see what we get.
Let's curl the reverse-proxy address (using `-k` flag to ignore warnings about insecure HTTP connection).

``` bash
curl -k http://172.17.0.2/
```

We're getting reverse-proxy's nginx page.
Now let's curl the same address but with `/merlin` at the end.

``` bash
curl -k http://172.17.0.2/merlin
```

and now we're getting the attacker's website!
![console prtsc](/img/remote/curl-reverseproxy-merlin.png)

The proxy successfully passed the request to and from our attacker!