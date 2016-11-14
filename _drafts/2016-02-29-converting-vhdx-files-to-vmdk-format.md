---
layout: post
title:  "Converting VHDX files to VMDK format"
date:   2016-02-29 20:00:00
categories: s3 jekyll
---

I was happily serving all of my VM's using Hyper-V but I need more GPU grunt for some dev work that I'm planning to do so I need to re-purpose the machine to make it more dev friendly. Part of that process involved swapping out the hypervisor in favour of VMWare's Workstation.


I found [this tool](https://www.starwindsoftware.com/converter) which worked really well for converting the disk format. Once I had the vmdk all I had to do was create the VM using the advanced wizard, without a disk, then attached the disk to one of the SCSI interfaces before boot.


