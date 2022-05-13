---
title: This is my seventh post.
description: Draft
date: 2021-06-28
scheduled: 2021-06-28
layout: layouts/post.njk
draft: true
---

# Taking the first step

Docker is an open-source project for automating the deployment of applications as portable, self-sufficient containers that can run on the cloud or on-premises.

A container is a standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another. A Docker container image is a lightweight, standalone, executable package of software that includes everything needed to run an application: code, runtime, system tools, system libraries and settings.

Go ahead and download [Docker](https://docker.com). You'll have to create a free account for that, it takes barely a couple of seconds.

We can create containers based on their images. You'll find a great collection on them on [DockerHub](https://hub.docker.com/).
For this project we'll be using a very lightweight linux distro [Alpine](https://hub.docker.com/_/alpine) for our Victim, [NGINX](https://hub.docker.com/_/nginx) for our Reverse-Proxy and [Kali linux](https://hub.docker.com/r/kalilinux/kali-rolling) for our attacker.


```
docker pull alpine
docker pull nginx:alpine
docker pull kalilinux/kali-rolling
```
![prtsc of a console](https://cdn.glitch.global/b39fd45a-08ef-47bc-a9ef-6aa62020936c/wolfff%40wolfberry_%20~%2010_05_2022%2015_35_01.png?v=1652189714734)

Check your images with:
```
docker images
```

Then we'll build our images. While we build we can name them for easier handling.

```
docker run --name victim alpine
docker run --name reverse-proxy nginx
docker run --name attacker klilinux/kalirolling 
```
You can check the containers that we've created with ```docker ps -a```.
![prtsc of a console](https://cdn.glitch.global/b39fd45a-08ef-47bc-a9ef-6aa62020936c/wolfff%40wolfberry_%20~%2010_05_2022%2015_31_45.png?v=1652189621877)

Next, we'll start our servers.