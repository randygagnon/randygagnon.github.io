---
title: Learn how to write Python in an AWS Cloud9 for free!
layout: post
categories: [DevOps,Python]
tags: [pfense,firewall,vpn,tailscale]
---

There probably isn't a better place to learn than on Cloud9. You won't have to worry about setting up your machine, installing dependencies, updates, etc. Just spin up an instance in the free tier and away you go!

**Prereq's:**

1. AWS Account
2. There is no second prereq. All you need is an account that works.

## Let's deploy an environment

Open up AWS and select or search for **Cloud9**
<video autoplay="autoplay" loop="loop" width="768" height="512">
    <source src="/assets/images/open-cloud9.webm" type="video/webm">
</video>
And click **Create environment**. Pick whatever you want for the name and description and leave the rest default. Click **Create.**
<video autoplay="autoplay" loop="loop" width="768" height="512">
    <source src="/assets/images/create-new-c9-instance.webm" type="video/webm">
</video>
It will take a few minutes to create the environment so go grab a coffee. Next, we'll open it up and start writing python!

Now open the environment and create a new **hello.py** file and paste the contents of this code:

```python
import sys

print('Hello, World!')

print('The sum of 2 and 3 is 5.')

sum = int(sys.argv[1]) + int(sys.argv[2])

print('The sum of {0} and {1} is {2}.'.format(sys.argv[1], sys.argv[2], sum))
```

<video autoplay="autoplay" loop="loop" width="768" height="512">
  <source src="/assets/images/open-cloud9-instance.webm" type="video/webm">
</video>

## Go ahead and run it

If you're running python without arguments you can use the green **Run** button, but we want to feed two numbers into the script: 5, and 9.

Click **Run** > **Run Configurations** > **New run configurations** and in the command box, paste this:

`hello.py 5 9`

And hit **Run**
<video autoplay="autoplay" loop="loop" width="768" height="512">
  <source src="/assets/images/run-hello.py.webm" type="video/webm">
</video>
Congratulations! You've run a Python script in the cloud! 

## Make your code do something

![do something](/assets/images/do-something.png)

To add functionality to Python we can install modules such as **boto3**. [Boto3 is an SDK written by AWS](https://boto3.amazonaws.com/v1/documentation/api/latest/guide/quickstart.html) that allows Python to interface with the AWS API. This lets us use Python to make AWS do things for us like... Create S3 buckets!

In the bottom half of the IDE, switch over to the bash tab and check that pip is installed(which it is), and then install boto3 with:

```bash
sudo python3 -m pip install boto3  # Install boto3 for Python 3.

python -m pip show boto3            # Verify boto3 is installed for the current version of Python.
```

<video autoplay="autoplay" loop="loop" width="768" height="512">
  <source src="/assets/images/install-boto3.webm" type="video/webm">
</video>
Now create a new file called **s3.py** and paste the following into it

```python
import sys
import boto3
from botocore.exceptions import ClientError


def list_my_buckets(s3_resource):
    print('Buckets:\n\t', *[b.name for b in s3_resource.buckets.all()], sep="\n\t")


def create_and_delete_my_bucket(s3_resource, bucket_name, keep_bucket):
    list_my_buckets(s3_resource)

    try:
        print('\nCreating new bucket:', bucket_name)
        bucket = s3_resource.create_bucket(
            Bucket=bucket_name,
            CreateBucketConfiguration={
                'LocationConstraint': s3_resource.meta.client.meta.region_name
            }
        )
    except ClientError as e:
        print(f"Couldn't create a bucket for the demo. Here's why: "
                f"{e.response['Error']['Message']}")
        raise

    bucket.wait_until_exists()
    list_my_buckets(s3_resource)

    if not keep_bucket:
        print('\nDeleting bucket:', bucket.name)
        bucket.delete()

        bucket.wait_until_not_exists()
        list_my_buckets(s3_resource)
    else:
        print('\nKeeping bucket:', bucket.name)


def main():
    import argparse

    parser = argparse.ArgumentParser()
    parser.add_argument('bucket_name', help='The name of the bucket to create.')
    parser.add_argument('region', help='The region in which to create your bucket.')
    parser.add_argument('--keep_bucket', help='Keeps the created bucket. When not '
                                                'specified, the bucket is deleted '
                                                'at the end of the demo.',
                        action='store_true')

    args = parser.parse_args()
    s3_resource = (
        boto3.resource('s3', region_name=args.region) if args.region
        else boto3.resource('s3'))
    try:
        create_and_delete_my_bucket(s3_resource, args.bucket_name, args.keep_bucket)
    except ClientError:
        print('Exiting the demo.')


if __name__ == '__main__':
    main()
```

<video autoplay="autoplay" loop="loop" width="768" height="512">
  <source src="/assets/images/create-s3-paste-code.webm" type="video/webm">
</video>

Create a new Run Configuration like we did before and use this as the command, but make sure to replace the bucket name with something completely unique. Just throw some jibberish in there, it's fine.

`s3.py my-new-bucket-as8ifne6y3cj us-east-2`

Go ahead and run the code

## A few notes

Boto3 has a strange behavior where it will create a bucket in your default region when you don't specify **locationconstraint**. But our code does specify the constraint so if your default region is us-east-1, then set us-east-2 as your s3 region. [Here is some documentation](https://boto3.amazonaws.com/v1/documentation/api/latest/guide/s3-example-creating-buckets.html) where they account for this.

Here is what the code is supposed to do:

1. Lists all the existing buckets before it creating anything
2. Creates the bucket and shows you the new one
3. It will delete the S3 bucket immediately before exiting - This is expected

<video autoplay="autoplay" loop="loop" width="768" height="512">
  <source src="/assets/images/run-the-code.webm" type="video/webm">
</video>

Congrats! You've now completed the bonus exercise of having your python code actually do something useful! Pat yourself on the back, cause next up we'll do something even better. Maybe a lambda?

![you're awesome](/assets/images/whos-awesome.png)
