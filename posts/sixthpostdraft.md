---
title: Package capture.
description: Draft
date: 2021-07-03
scheduled: 2021-07-03
layout: layouts/post.njk
tags:
    - posts
---

It's time to capture our traffic with a tool called ![tcpdump](https://www.tcpdump.org/) so we can analyze it later with ![WireShark](https://www.wireshark.org/).

Let's get tcpdump on our victim.

``` bash
apt install tcpdump
```

And let's see how it works. Open up another terminal and log into Victim.

``` bash
docker exec -it victim /bin/bash
```

Let's start the capture in one of the Victim terminals:

``` bash
tcpdump -s 65535 -i any -v -w my_capture.pcap
```

- s: Older versions of Tcpdump cut-off packets to 68 or 96 bytes. The ‘-s’ option is used for capturing packets with full length.
- i: Selects the interface to listen on.
- w: Saves the raw packets captured to a file instead of displaying them on the terminal. 

In the other Victim terminal let's ping the reverse-proxy and visit main reverse-proxy page:

![ping and curl test](/img/remote/ping-curl-test.png)
![tcpdump test](/img/remote/tcpdump-test.png)

>You can stop the capture with CTRL + C.

Now that we have our capture file, we can transfer it to our Desktop and read it with WireShark.
Get the ID of your Victim container with `docker ps` and move the file like we did with our agent:

![move the capture](/img/remote/move-capture.png)

Now it's time to download ![WireShark](https://www.wireshark.org/#download).
WireShark is a great network protocol analyzer and will let us see deep into our captured conversations.
When you install it, open the my_capture.pcap file.

![capture](/img/remote/my-capture.png)

You can see so many cool things!

Ping sends a request and then receives a reply!
You can see the handshake happen when we curled the reverse-proxy server, and the key exchange!
We can inspect the encrypted packets!

Truly WireShark is a marvel.