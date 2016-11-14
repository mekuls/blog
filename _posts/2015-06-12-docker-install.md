---
layout: post
title:  "Docker Install on Ubuntu 14.04"
date:   2015-06-12 10:00:00
categories: install docker
---

I was recently playing with docker to see what all the fuss was about. This post has the basic commands that got me up and running.


#Setting up docker on Ubuntu 
Here are the steps I used to get started with Docker on my dev machine

Firstly install the docker

{% highlight bash %}
apt-get update && apt-get install -y docker.io
{% endhighlight %}

Create a link in /usr/bin and fix bash autocompletion config.

{% highlight bash %}
ln -sf /usr/bin/docker.io /usr/local/bin/docker
sed -i '$acomplete -F _docker docker' /etc/bash_completion.d/docker
{% endhighlight %}

Have Docker start on boot

{% highlight bash %}
update-rc.d docker.io defaults
{% endhighlight %}

