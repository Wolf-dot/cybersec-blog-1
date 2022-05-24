---
title: Now why are we here?
description: Draft
date: 2021-06-32
scheduled: 2021-06-32
layout: layouts/post.njk
tags:
    - posts
---

We have our Attacker server ready,
we have our Reverse-Proxy ready,
and we have encrypted HTTPS communication.

It's time for our RAT.
A RAT is a Remote Access Tool. They can be used in a number of positive ways like Windows Remote Control or SSH sessions.
Our RAT is going to be malicious though. We're going to control our Victim's system from our Attacker and by using a Reverse-Proxy
and SSL we'll be masked and hidden, impossible to trace. Well, not really. But it won't be easy!

First let's get our tools.
Download wget:

```bash 
apt install wget
```