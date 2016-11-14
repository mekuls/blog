Offloading wordpress file serving

**IMPORTANT**
For me, this never quite worked, if you're keen to offload your wordpress asset serving you're better off fixing your wordpress deployment and replacing stored images with s3 url's by design.


Setting up an s3 bucket.

1. Create a new bucket.
2. Create a new user 
3. Create a new IAM Policy
4. Call the policy s3 rw BUCKETNAME or something similar
5. Use the following policy document

Bucket permissions:

{% highlight %}
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "s3:*",
            "Resource": [
                "arn:aws:s3:::BUCKET_NAME/",
                "arn:aws:s3:::BUCKET_NAME/*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::BUCKET_NAME"
            ],
            "Condition": {}
        }
    ]
}
{% endhighlight %}

6. Open the policy and choose the tab "Attached Entities" and attach the policy to the user.

Test access to the bucket using 

{% highlight %}
export AWS_ACCESS_KEY_ID=AKIAIOSFODNN7EXAMPLE
export AWS_SECRET_ACCESS_KEY=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
export AWS_DEFAULT_REGION=us-west-2
{% endhighlight %}

If you aren't sure of the correct code for your endpoint use the following webpage as a reference.

[http://docs.aws.amazon.com/general/latest/gr/rande.html]

**Quick s3 reference for testing:**

{% highlight %}
touch file.txt
aws s3 cp file.txt s3://BUCKETNAME/
aws s3 ls s3://BUCKETNAME/
aws s3 rm s3://BUCKETNAME/file.txt
{% endhighlight %}


**Sync using ansible.**

*HINT: To obtain a full list of file extensions in a given directory use:*

{% highlight %}
 `find . -type f | sed 's|.*\.||' | sort -u`
{% endhighlight %}

Task:
{$ highlight yaml %}
---
- name: "syncing content to s3 bucket"
  shell: "aws s3 sync {{source_path}} s3://{{aws_bucket_name}} --delete {{sync_args}}"
  environment:
    AWS_ACCESS_KEY_ID: '{{aws_access_key_id}}'
    AWS_SECRET_ACCESS_KEY: '{{aws_secret_access_key}}'
    AWS_DEFAULT_REGION: '{{aws_bucket_region}}'
  async: 3600
  poll: 0
  tags: ['sync']
{% endhighlight %}

Using `group_vars` similar to the following:

{% highlight %}
aws_bucket_name: "your bucket name"
aws_bucket_region: "your region"
aws_access_key_id: "your access key"
aws_secret_access_key: "your secret"
sync_args: >
    --exclude '*' 
    --include '*.gif' 
    --include '*.ico' 
    --include '*.jpeg' 
    --include '*.jpg' 
    --include '*.mp3' 
    --include '*.mp4' 
source_path: "/var/www/path/to/wordpress/wp-content"
{% endhighlight %}

Note that due to the 'async' parameter the playbook will return, you'll need to ssh into your server and use ps to manage the process if need be.
