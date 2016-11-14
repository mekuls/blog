---
layout: post
title:  "Installing Hyper-V integration tools on an Ubuntu 14.04 guest"
date:   2015-11-24 09:00:00
categories: hyper-v windows ubuntu 
---


{% highlight bash %}
sudo apt-get update
sudo apt-get install -y hv-kvp-daemon-init linux-tools-virtual linux-cloud-tools-virtual
{% endhighlight %}

https://technet.microsoft.com/en-au/library/dn531029.aspx

Also note that for Gen2 VM's, secure boot must be disabled



{% highlight powershell %}
Set-VMFirmware –VMName "VMname" -EnableSecureBoot Off
{% endhighlight %}