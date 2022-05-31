---
title: Configuring servers.
description: Draft
date: 2021-06-29
scheduled: 2021-06-29
layout: layouts/post.njk
order: 2
tags:
    - posts
---

Now we're going to configure the basic reverse-proxy website.
Let's see how it looks now, run:

``` bash
curl localhost
```
![console prtsc](/img/remote/curl-localhost-proxy.png)

We're getting a default nginx website.
Let's now navigate to `/etc/nginx/conf.d/`

`ls` will list items in the current directory.
`cd etc` will change directory to etc. 
Use `cd ..` to go up (back) in directories.

Inside you'll find `default.conf` file. Let's edit it with nano.

``` bash
nano default.conf
```

That's the default configuration. We won't be making a lot of changes.
For now lets copy the contents. To copy you can highlight the contents and right-click.

### Attacker
Navigate to the same directory in the attacker container, then create a file in `conf.d` useing the following command:

``` bash
touch default.conf
```
Right click to paste what we copied earlier.
Now we'll change the port at which we'll listen for connections.
To exit nano you use `ctrl+x`, then type 'y' to confirm and enter.
Now that we have the default configuration let's modify the `index.html` a little to make it clear if we get the attacker's website.
Navigate to `/usr/share/nginx/html`.
Open index.html and let's make it really simple:

``` html
<!DOCTYPE html>
<html>
<head>
<title>Welcome to Merlin!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to MERLIN!</h1>
</body>
</html>
```

Let's also go to `/etc/nginx/sites-enabled` and delete the `default` file since we're using `default.conf` file that we created earlier.

``` bash
rm default
```
Now let's reload nginx and see if it works.

``` bash
nginx -s reload
curl localhost
```

![console prtsc](/img/remote/curl-localhost-attacker.png)

**It works!**
But if it didn't for you, try the following:

``` bash
service nginx restart
nginx -s reload -t
```

Now let's see if we can see this page from the Victim's container.
First, check Attacker's ip with:

``` bash
ip addr
```
For me it's `172.17.0.3`
Then in Victim's terminal type in:

``` bash
curl http://172.17.0.3/
```

**And it works again!**