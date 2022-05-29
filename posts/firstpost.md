---
title: Creating Docker containers.
date: 2021-06-28
scheduled: 2021-06-28
layout: layouts/post.njk
tags:
    - posts
---

Docker is an open-source project for automating the deployment of applications as portable, self-sufficient containers that can run on the cloud or on-premises.

A container is a standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another. A Docker container image is a lightweight, standalone, executable package of software that includes everything needed to run an application: code, runtime, system tools, system libraries and settings.

Go ahead and download [Docker](https://docker.com). You'll have to create a free account for that, but it takes barely a couple of seconds.

We can create containers based on their images. You'll find a great collection of them on [DockerHub](https://hub.docker.com/).
For this project we'll be using [Kali linux](https://hub.docker.com/r/kalilinux/kali-rolling) for our *Attacker* and *Victim* and [NGINX](https://hub.docker.com/_/nginx) for our *Reverse-Proxy*.


``` bash
docker pull nginx:alpine
docker pull kalilinux/kali-rolling
```

![prtsc of a console](/img/remote/pull-images.png)

Check your images with:

```bash
docker images
```

Then we'll build our images. While we build we can name them for easier handling.

``` shell
docker run --name victim klilinux/kalirolling 
docker run --name reverse-proxy nginx
docker run --name attacker klilinux/kalirolling 
```

You can check the containers that we've created with `docker ps -a`.
![prtsc of a console](https://cdn.glitch.global/b39fd45a-08ef-47bc-a9ef-6aa62020936c/wolfff%40wolfberry_%20~%2010_05_2022%2015_31_45.png?v=1652189621877)

Now let's install some packages, but first let's update and upgrade.
>Kali Linux uses `apt` as package manager so that's what we'll use for attacker and victim commands, but our reverse-proxy runs on Alpine Linux which uses `apk` so be sure to use that instead.

``` bash
apt update && upgrade
```

Every container will need ![curl](https://curl.se/docs/manpage.html) to reach out to servers and get conent back as well as ![iproute2]() so we can check our IP and ![iputils-ping]() to check if our containers are well connected. We'll also need ![nano](https://www.nano-editor.org/) for our attacker and reverse-proxy, it's a text editor.

For everyone:

``` bash
apt install curl
apt install iproute2
apt install iputils-ping
```

For attacker and reverse-proxy:

``` bash
apt install nano
```

Docker has a default network called bridge which it will automatically create between your containers unless specified otherwise. That means that we can easily communicate between our containers!
>The IP address that each container gets will change with each reload, so remember to check and adjust if you're making this project over several days.
Let's check our IP addresses using `ip addr`.

![Victim ipaddr](/img/remote/victim-ipaddr.png)

For my victim container it's `172.17.0.2`.
Now if you have IPs for all three, try to ping them from each container to see if they're connected.
You can stop the ping with CTR + C.

![revrse-proxy ping](/img/remote/reverse-proxy-ping.png)

Next we'll start our servers!

>If you exit your container and want to get back into it use the command `docker exec -it victim /bin/bash`. If you stopped it you have to start it up again with `docker start victim`.