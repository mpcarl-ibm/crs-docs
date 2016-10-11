---
layout: page
title:  "Tools and SDKs"
featured: true
weight: 1
tags: []
author: Nick
dateAdded: August 18th, 2016
---

> IBM COS Standard Cross Region is currently in beta, and only available to registered users.  Please contact [IBM sales](mailto:insidesales@cleversafe.com?subject=Beta Request) to enroll.

Most tools that use the S3 API to interact with object storage are compatible with IBM COS, although some configuration is necessary to connect to the correct endpoint.  

# Command line tools

#### AWS CLI
The official command line interface for AWS is compatible with the IBM COS S3 API. Written in Python, it can be installed from the Python Package Index via `pip install awscli`. Access keys are sourced from `~/.aws/credentials`.

Minimum required `~/.aws/credentials` file:

```
[default]
aws_access_key_id = {Access Key ID}
aws_secret_access_key = {Secret Access Key}
```

The IBM COS endpoint must be sourced using the `--endpoint-url` option, and can not be set in the credentials file.

Simple use cases can be accomplished using `aws --endpoint-url={endpoint} s3 <command>`. Objects are managed using familiar shell commands, such as `ls`, `mv`, `cp`, and `rm`.  Buckets can be deleted using `rb`.  

**Note**: Buckets can not be created using this method, but require using the `s3api` method described below.

Listing buckets:

```
$ aws --endpoint-url=https://{endpoint} s3 ls 
2016-09-09 12:48  s3://bucket-1
2016-09-16 21:29  s3://bucket-2
```

Listing objects within a bucket:

```bash
$ aws --endpoint-url=https://{endpoint} s3 ls s3://bucket-1/
2016-09-28 15:36       837   s3://bucket-1/c1ca2-filename-00001
2016-09-09 12:49       533   s3://bucket-1/c9872-filename-00002
2016-09-28 15:36     14476   s3://bucket-1/98837-filename-00003
2016-09-29 16:24     20950   s3://bucket-1/abfc4-filename-00004
```

Uploading a new object to a bucket:

```bash
$ aws --endpoint-url=https://{endpoint} s3 cp /tmp/new-file s3://bucket-1/
upload: /tmp/new-file to s3://bucket-1/new-file
```

Copying an object from one bucket to another:

```bash
$ aws --endpoint-url=https://{endpoint} s3 cp s3://bucket-1/new-file s3://bucket-2/
copy: s3://bucket-1/new-file to s3://bucket-2/new-file
```

More complex operations require leveraging the api directly with `aws --endpoint-url={endpoint} s3api <command>`, which provides a standard JSON reponse.  

Listing buckets:

```bash
$ aws --endpoint-url=https://{endpoint} s3api list-buckets
{
    "Owner": {
        "DisplayName": "{Access Key ID}",
        "ID": "{Access Key ID}"
    },
    "Buckets": [
        {
            "CreationDate": "2016-09-09T12:48:52.442Z",
            "Name": "bucket-1"
        },
        {
            "CreationDate": "2016-09-16T21:29:00.912Z",
            "Name": "bucket-2"
        }
    ]
}
```

Creating a new bucket:

```bash
$ aws --endpoint-url=https://{endpoint} s3api create-bucket --bucket {bucket-name}
```

Listing objects within a bucket:

```bash
$ aws --endpoint-url=https://{endpoint} s3api list-objects --bucket bucket1
{
    "Contents": [
        {
            "LastModified": "2016-09-28T15:36:56.807Z",
            "ETag": "\"13d567d518c650414c50a81805fff7f2\"",
            "StorageClass": "STANDARD",
            "Key": "c1ca2-filename-00001",
            "Owner": {
                "DisplayName": "{Access Key ID}",
                "ID": "{Access Key ID}"
            },
            "Size": 837
        },
        {
            "LastModified": "2016-09-09T12:49:58.018Z",
            "ETag": "\"3ca744fa96cb95e92081708887f63de5\"",
            "StorageClass": "STANDARD",
            "Key": "c9872-filename-00002",
            "Owner": {
                "DisplayName": "{Access Key ID}",
                "ID": "{Access Key ID}"
            },
            "Size": 533
        },
        {
            "LastModified": "2016-09-28T15:36:17.573Z",
            "ETag": "\"a54ed08bcb07c28f89f4b14ff54ce5b7\"",
            "StorageClass": "STANDARD",
            "Key": "98837-filename-00003",
            "Owner": {
                "DisplayName": "{Access Key ID}",
                "ID": "{Access Key ID}"
            },
            "Size": 14476
        },
        {
            "LastModified": "2016-10-06T14:46:26.923Z",
            "ETag": "\"2bcc8ee6bc1e4b8cd2f9a1d61d817ed2\"",
            "StorageClass": "STANDARD",
            "Key": "abfc4-filename-00004",
            "Owner": {
                "DisplayName": "{Access Key ID}",
                "ID": "{Access Key ID}"
            },
            "Size": 20950
        }
    ]
}
```

# Python

>In progress.

# Java

>In progress.

# Go

>In progress.

# .NET

Use of the .NET SDK is not recommended at this time as it invokes undocumented APIs on IBM COS. 