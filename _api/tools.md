---
layout: page
title:  "Tools and SDKs"
featured: true
weight: 2
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

Python support is provided through the `boto3` library.  It can be installed from the Python Package Index via `pip install boto3`. 

> **Note**: Existing applications that use the original `boto` library should be compatible as well, although `boto` is no longer being actively supported and users are encouraged to migrate to `boto3`. 

The `boto3` library provides complete access to the S3 API and can source credentials from the `~/.aws/credentials` file referenced above.  The IBM COS endpoint must be specified in the `boto3.client()` method as shown in the following example. The `pprintpp` package is used for readability of raw output.

Detailed documentation can be found at [boto3.readthedocs.io](https://boto3.readthedocs.io).

**Sample script**

```python
import boto3
import pprintpp

endpoint = 'https://s3-api.us-geo.objectstorage.softlayer.net'

s3 = boto3.client('s3', endpoint_url=endpoint)

buckets = s3.list_buckets()

print('=') * 110
print('These are the buckets in this service account:')
bucketNames = buckets['Buckets']
pprintpp.pprint(buckets, width=180)
print('=') * 110


for bucket in buckets['Buckets']:
    name = bucket['Name']
    print("Raw output from 'list_buckets()' in %s:" % name)
    objects = s3.list_objects(Bucket=name)
    pprintpp.pprint(objects)
    print('-') * 110
```

**Sample script response**

```bash
==============================================================================================================
These are the buckets in this service account:
{
    u'Buckets': [
        {u'CreationDate': datetime.datetime(1970, 1, 1, 0, 0, tzinfo=tzutc()), u'Name': 'bucket-1'},
        {u'CreationDate': datetime.datetime(2016, 9, 16, 21, 29, 0, 912000, tzinfo=tzutc()), u'Name': 'bucket-2'},
    ],
    u'Owner': {u'DisplayName': '{Access Key ID}', u'ID': '{Access Key ID}'},
    'ResponseMetadata': {
        'HTTPHeaders': {
            'accept-ranges': 'bytes',
            'content-length': '463',
            'content-type': 'application/xml',
            'date': 'Wed, 12 Oct 2016 13:53:27 GMT',
            'server': 'Cleversafe/3.9.0.129',
            'x-amz-request-id': '0a7a3f3b-d788-45c6-a16d-9025031e43cb',
            'x-clv-request-id': '0a7a3f3b-d788-45c6-a16d-9025031e43cb',
            'x-clv-s3-version': '2.5',
        },
        'HTTPStatusCode': 200,
        'HostId': '',
        'RequestId': '0a7a3f3b-d788-45c6-a16d-9025031e43cb',
        'RetryAttempts': 0,
    },
}
==============================================================================================================
Raw output from 'list_buckets()' in apiary:
{
    u'Contents': [
        {
            u'ETag': '"2bcc8ee6bc1e4b8cd2f9a1d61d817ed2"',
            u'Key': 'c1ca2-filename-00001',
            u'LastModified': datetime.datetime(2016, 10, 6, 14, 44, 37, 211000, tzinfo=tzutc()),
            u'Owner': {
                u'DisplayName': '{Access Key ID}',
                u'ID': '{Access Key ID}',
            },
            u'Size': 20950,
            u'StorageClass': 'STANDARD',
        },
        {
            u'ETag': '"3ca744fa96cb95e92081708887f63de5"',
            u'Key': 'c9872-filename-00002',
            u'LastModified': datetime.datetime(2016, 10, 11, 13, 12, 32, 234000, tzinfo=tzutc()),
            u'Owner': {
                u'DisplayName': '{Access Key ID}',
                u'ID': '{Access Key ID}',
            },
            u'Size': 533,
            u'StorageClass': 'STANDARD',
        },
        {
            u'ETag': '"ed5204ca0be62274dcd4dca3a37ae606"',
            u'Key': '98837-filename-00003',
            u'LastModified': datetime.datetime(2016, 10, 6, 15, 11, 49, 20000, tzinfo=tzutc()),
            u'Owner': {
                u'DisplayName': '{Access Key ID}',
                u'ID': '{Access Key ID}',
            },
            u'Size': 1989,
            u'StorageClass': 'STANDARD',
        },
        {
            u'ETag': '"13d567d518c650414c50a81805fff7f2"',
            u'Key': 'abfc4-filename-00004',
            u'LastModified': datetime.datetime(2016, 9, 28, 15, 36, 56, 807000, tzinfo=tzutc()),
            u'Owner': {
                u'DisplayName': '{Access Key ID}',
                u'ID': '{Access Key ID}',
            },
            u'Size': 837,
            u'StorageClass': 'STANDARD',
        },
    ],
    u'Delimiter': '',
    u'IsTruncated': False,
    u'Marker': '',
    u'MaxKeys': 1000,
    u'Name': 'bucket-1',
    u'Prefix': '',
    'ResponseMetadata': {
        'HTTPHeaders': {
            'accept-ranges': 'bytes',
            'content-length': '2492',
            'content-type': 'application/xml',
            'date': 'Wed, 12 Oct 2016 13:53:27 GMT',
            'server': 'Cleversafe/3.9.0.129',
            'x-amz-request-id': 'bb6984e9-68bf-4419-be98-1af0dbc782f9',
            'x-clv-request-id': 'bb6984e9-68bf-4419-be98-1af0dbc782f9',
            'x-clv-s3-version': '2.5',
        },
        'HTTPStatusCode': 200,
        'HostId': '',
        'RequestId': 'bb6984e9-68bf-4419-be98-1af0dbc782f9',
        'RetryAttempts': 0,
    },
}
--------------------------------------------------------------------------------------------------------------
Raw output from 'list_buckets()' in bucket-2:
{
    u'Contents': [
        {
            u'ETag': '"88bfd87922eb17cff5b791c4d3fee1e4"',
            u'Key': 'c1ca2-filename-00011',
            u'LastModified': datetime.datetime(2016, 9, 17, 18, 11, 58, 811000, tzinfo=tzutc()),
            u'Owner': {
                u'DisplayName': '{Access Key ID}',
                u'ID': '{Access Key ID}',
            },
            u'Size': 112001,
            u'StorageClass': 'STANDARD',
        },
    ],
    u'Delimiter': '',
    u'IsTruncated': False,
    u'Marker': '',
    u'MaxKeys': 1000,
    u'Name': 'bucket-2',
    u'Prefix': '',
    'ResponseMetadata': {
        'HTTPHeaders': {
            'accept-ranges': 'bytes',
            'content-length': '905',
            'content-type': 'application/xml',
            'date': 'Wed, 12 Oct 2016 13:53:27 GMT',
            'server': 'Cleversafe/3.9.0.129',
            'x-amz-request-id': 'ac14085f-2e96-4c5a-b276-de98edb32e30',
            'x-clv-request-id': 'ac14085f-2e96-4c5a-b276-de98edb32e30',
            'x-clv-s3-version': '2.5',
        },
        'HTTPStatusCode': 200,
        'HostId': '',
        'RequestId': 'ac14085f-2e96-4c5a-b276-de98edb32e30',
        'RetryAttempts': 0,
    },
}
```
# Java

>In progress.

# Go

>In progress.

# node.js

>In progress.

# .NET

Use of the .NET SDK is not recommended at this time as it invokes undocumented APIs on IBM COS. 