---
title: Now why are we here?
description: Draft
date: 2021-07-01
scheduled: 2021-07-01
layout: layouts/post.njk
tags:
    - posts
---

It's time to capture our traffic with a tool called ![tcpdump](https://www.tcpdump.org/) so we can analyze it later with ![WireShark](https://www.wireshark.org/).

Let's get tcpdump on our victim.

``` bash
apt install tcpdump
```




// two? scenarios for capture
// - no reverse proxy http
// - https with reverse proxy