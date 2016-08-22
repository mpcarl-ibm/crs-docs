---
layout: page
title:  "API Reference & Examples"
featured: true
weight: 3
tags: []
author: Nick
dateAdded: August 18th, 2016
---

#### Table of Contents

* [Overview](#overview)
* [Common Headers and Error Responses](#headers-and-error-response)
* [Operations on Service](#operations-on-service)
* [Operations on Containers](#operations-on-buckets)
* [Operations on Objects](#operations-on-objects)

###  Overview
{: #overview}

The IBM Cloud Object Storage S3 API supports the most commonly used subset of Amazon S3 API operations. Any undocumented S3 methods are unsupported.

### Common Headers and Error Responses
{: #headers-and-error-response} 

#### Common Request Headers
The following are the common request headers supported by COS. Unsupported headers will be ignored if sent in a request.

| Header             | Note                               |
|--------------------|-------------------------------------|
| Authorization      |  AWS, AWS4, or BASIC authentication. |
| Content-Length     | Chunked encoding also supported.    |
| Content-Type       | Stored as object metadata.          |
| Content-MD5        |                                  |
| Date               |                                     |
| Expect             |                                    | 
| Host               |                                     |
|x-amz-content-sha256| Used with signature version 4 authenticated requests. |
| x-amz-date         |                                    |



####  Common Response Headers
The following are the common response headers supported from the S3 API.

|  Header        | Note |
|----------------|------|
| Content-Length |      |
|Connection     |       | 
| Date           |       |
| ETag           |  Contains MD5 checksum. |
| Server         |      | 
|x-amz-request-id|       |
|X-Clv-Request-Id|  Unique identifier per response an IBM COS Support Engineer uses for diagnostics and troubleshooting purposes. |
|x-amz-version-id|      |
|X-Clv-S3-Version|  S3 version  used for the request. |

#### Error Responses

In addition, the following COS API unique response code might be used.

| HTTP Status Code | Description  |  Error Code  |
|-------------|--------------|-------------------|
| 507 | The capacity used on the target bucket has exceeded a hard quota | VaultQuotaExceeded |




### Operations on Service
{: #operations-on-service}

#### GET list of containers

A `GET` issued to the endpoint root returns a list of containers.

**Sample Request:**

~~~
GET / HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Host: s3-api.{endpoint}.objectstorage.softlayer.net
X-Amz-Date: 20160822T030815Z
Authorization: {authorization-string}
~~~

**Sample Response:**

~~~
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<ListAllMyBucketsResult xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
    <Owner>
        <ID>7dd4f57d38484131999e6739e40279a7</ID>
        <DisplayName>7dd4f57d38484131999e6739e40279a7</DisplayName>
    </Owner>
    <Buckets>
        <Bucket>
            <Name>bucket-27200-lwx4cfvcue</Name>
            <CreationDate>2016-08-18T14:21:36.593Z</CreationDate>
        </Bucket>
        <Bucket>
            <Name>bucket-27590-drqmydpfdv</Name>
            <CreationDate>2016-08-18T14:22:32.366Z</CreationDate>
        </Bucket>
        <Bucket>
            <Name>bucket-27852-290jtb0n2y</Name>
            <CreationDate>2016-08-18T14:23:03.141Z</CreationDate>
        </Bucket>
        <Bucket>
            <Name>bucket-28731-k0o1gde2rm</Name>
            <CreationDate>2016-08-18T14:25:09.599Z</CreationDate>
        </Bucket>
    </Buckets>
</ListAllMyBucketsResult>
~~~

### Operations on Buckets
{: #operations-on-buckets}

#### PUT Bucket
**Sample Request:**

~~~
PUT /{bucket-name} HTTP/1.1
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW
Host: s3-api.{endpoint}.objectstorage.softlayer.net
X-Amz-Date: 20160821T052842Z
Authorization:{authorization-string}
~~~

#### DELETE Bucket
**Sample Request**

~~~
DELETE /{bucket-name} HTTP/1.1
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW
Host: s3-api.{endpoint}.objectstorage.softlayer.net
X-Amz-Date: 20160822T064812Z
Authorization: {authorization-string}
~~~

The server responds with `204 No Content`.


#### GET Bucket (list objects)

When a `GET` request is given to a specific container, a list of the contents are returned.

**Sample Request**

~~~
GET /{bucket-name} HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Host: s3-api.{endpoint}.objectstorage.softlayer.net
X-Amz-Date: 20160822T225156Z
Authorization: {authorization-string}
~~~

**Sample Response**

~~~
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<ListBucketResult xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
    <Name>{bucket-name}</Name>
    <Prefix></Prefix>
    <Marker></Marker>
    <MaxKeys>1000</MaxKeys>
    <Delimiter></Delimiter>
    <IsTruncated>false</IsTruncated>
    <Contents>
        <Key>object_1</Key>
        <LastModified>2016-08-18T16:27:32.530Z</LastModified>
        <ETag>"3c66102af4bdeb4dc7cc08b6d800b82b"</ETag>
        <Size>12</Size>
        <Owner>
            <ID>7dd4f57d38484131999e6739e40279a7</ID>
            <DisplayName>7dd4f57d38484131999e6739e40279a7</DisplayName>
        </Owner>
        <StorageClass>STANDARD</StorageClass>
    </Contents>
    <Contents>
        <Key>object_2</Key>
        <LastModified>2016-08-18T16:29:42.261Z</LastModified>
        <ETag>"329b59153a23e700f4121141facb6c57"</ETag>
        <Size>104912320</Size>
        <Owner>
            <ID>7dd4f57d38484131999e6739e40279a7</ID>
            <DisplayName>7dd4f57d38484131999e6739e40279a7</DisplayName>
        </Owner>
        <StorageClass>STANDARD</StorageClass>
    </Contents>
</ListBucketResult>
~~~


### Operations on Objects
{: #operations-on-objects}


#### GET Object

A `GET` given a path to an object, such as `{endpoint}/{bucket-name}/{object-name}` downloads the object.

**Sample Request**

~~~
GET /{bucket-name}/{object-name} HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Host: s3-api.{endpoint}.objectstorage.softlayer.net
X-Amz-Date: 20160822T230250Z
Authorization: {authorization-string}
~~~

**Sample Response Headers**

~~~
Accept-Ranges →bytes
Content-Length →12
Content-Type →application/octet-stream
Date →Mon, 22 Aug 2016 23:02:50 GMT
ETag →"3c66102af4bdeb4dc7cc08b6d800b82b"
Last-Modified →Thu, 18 Aug 2016 16:27:32 GMT
Server →Cleversafe/3.9.0.115
X-Clv-Request-Id →f11f3447-cfab-476b-b5da-fa7219d4c7bd
X-Clv-S3-Version →2.5
x-amz-request-id →f11f3447-cfab-476b-b5da-fa7219d4c7bd
~~~

#### HEAD Object




#### POST Object


**Parameters:**

| Form Field |
|------------|
| AWSAccessKeyId | 
| acl  | 
| key | 
| policy |
| success_action_status | 
| signature |
| Other field names prefixed with x-amz-meta- | 
| file | 


#### DELETE Object


**Parameters:**

| Operation | Header | 
|-----------|--------|
| Response Headers | x-amz-delete-marker | 


#### PUT Object

