---
layout: post
title:  "Compressing and saving files to Amazon S3 Using Powershell"
date:   2015-12-12 10:00:00
categories: s3 amazon powershell compression
---

Here's a little Gist that describes the process of 

1. Compressing a file from a powershell script without using 7zip (those days are now over)
2. Uploading the archive to an s3 bucket using the keys provided through IAM.

<script src="https://gist.github.com/lukemuccillo/134bbfd23802b5d0dc98.js"></script>

## Note:

You'll probably need something like this in order to be able to upload:

{% highlight json %}
{
  "Version": "2012-10-17",
  "Statement":[{
    "Effect": "Allow",
     "Action": "s3:*", 
    "Resource": "arn:aws:s3:::bucketname/*"
    }
  ]
} 
{% endhighlight %}