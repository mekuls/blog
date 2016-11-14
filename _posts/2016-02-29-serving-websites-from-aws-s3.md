---
layout: post
title:  "Serving static websites from an AWS S3 bucket"
date:   2016-02-29 20:00:00
categories: s3 jekyll
---

Static websites are the bomb, I would love to take this opportunity to flame on Wordpress but rather than waste time ranting I think I'll just wait for the world to catch up and realize that we don't need to go making webapps with databases in order to serve static business sites with five pages of marketing text, some images and some js.


This article describes the process that I went through in order to get the site served through Amazon S3.

1. Create a Bucket that has the name of the host that you're going to be serving, i.e. 'example.com'
2. Enable web hosting on the Bucket
3. Add a bucket policy to allow users to Read and Get objects from your bucket (See the bucket policy below)
4. Upload your website.
5. Add a bucket policy to enable unauthenticated users access to list and retrieve the site contents

## Here's the Bucket Policy that I used to allow users to read.

{% highlight json %}
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::skycoderlabs.com/*"
    }
  ]
}
{% endhighlight %}

## Jekyll permalinks

One thing that I had to tweak a little was the permalinks on my Jekyll site, in the config.yml file it is necessary to ensure that the permalink is something that ends in 'index.html' to ensure that the s3 bucket web server know's what to do.

eg
{% highlight json %}
{
  permalink: /:year/:month/:day/:title/index.html
}
{% endhighlight %}


## A little bit of additional automation.

I don't want to be manually dragging my files up to s3 each time I make a change, that's scriptable. On a windows box, a bit of modification to the the below gist will upload the site automatically. Note that in this code snippet we're clobbering the entire site so if you have something with a tonne of files in it you're going to need to find a more effective way of managing the upload process in order to minimise the number of PUT requests that you get charged for each month. Since my blog is still in it's infancy, this is a non issue for me.

<script src="https://gist.github.com/lukemuccillo/42830051b6a31e8eee53.js"></script>


## Access keys

1. Log in to IAM.
2. Create a new User (And generate an access key).
3. Create a new Policy and use the below json in the policy document.
4. Attach the freshly created policy to the user just created. Do this by: 
  * Finding the policy in the Policy list (click 'Policies' on the left nav) and clicking on it.
  * Going to the 'Attached Entities' tab.
  * Click 'Attach'.
  * Click on the dude.
  * Lock it in..

This is the IAM policy that I used to grant a user write permissions in order to automate site uploads. Note, this is full access permissions, I fooled around trying to get the exact permissions necessary but grew really bored and sledge hammered it.

{% highlight json %}
{
  "Version": "2012-10-17",
  "Statement":[{
    "Effect": "Allow",
     "Action": "s3:*", 
    "Resource": ["arn:aws:s3:::skycoderlabs.com/",
                 "arn:aws:s3:::skycoderlabs.com/*"]
    }
  ]
} 
{% endhighlight %}

# DNS stuff

It's pretty straight forward to get DNS working, I'm using Route 53 so it was just a matter of:

1. Selecting 'alias' on the A record for my domain.
2. Choosing the S3 bucket from the dropdown list

I have included a link below which has a pretty solid example that got me through the last bit.

## Further Reading

* [General web access permissions page](http://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteAccessPermissionsReqd.html)
* [Amazon Resource (ARN) page](http://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html)
* [IAM Policies in the context of S3](http://blogs.aws.amazon.com/security/post/TxPOJBY6FE360K/IAM-policies-and-Bucket-Policies-and-ACLs-Oh-My-Controlling-Access-to-S3-Resourc)
* [List of S3 Policy Operations](http://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html)
* [Solid S3 website example](https://docs.aws.amazon.com/AmazonS3/latest/dev/website-hosting-custom-domain-walkthrough.html#root-domain-walkthrough-s3-tasks)
