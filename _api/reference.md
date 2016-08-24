---
layout: page
title:  "API Reference & Examples"
featured: true
weight: 3
tags: []
author: Nick
dateAdded: August 18th, 2016
---

```** INTERNAL DRAFT NOTE: code examples and parameter details will be added continously for the set of calls listed here **```

#### Table of Contents

* [Overview](#overview)
* [Common Headers and Error Responses](#headers-and-error-response)
* [Operations on Service](#operations-on-service)
* [Operations on Containers](#operations-on-buckets)
* [Operations on Objects](#operations-on-objects)

###  Overview
{: #overview}

The IBM Cloud Object Storage S3 API supports the most commonly used subset of Amazon S3 API operations. A complete list of supported operations can be found [here]({{ site.baseurl }}/beta/api/overview/).

### Common Headers and Error Responses
{: #headers-and-error-response} 

#### Common Request Headers
The following table describes supported common request headers. Headers not listed here will be ignored if sent in a request.

| Header             | Note                               |
|--------------------|-------------------------------------|
| Authorization      |  AWS4 authentication. |
| Content-Length     | Chunked encoding also supported.    |
| Content-MD5        |                                  |
| Date               |                                     |
| Expect             |                                    | 
| Host               |                                     |
|x-amz-content-sha256| Used with signature version 4 authenticated requests. |
| x-amz-date         |                                    |



####  Common Response Headers
The following table describes common response headers.

```** INTERNAL DRAFT NOTE: ETag and X-Clv-S3-Version might be removed. X-Clv-Request-id should possibly be renamed to X-IBM-Request-id **```

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

### Operations on Service
{: #operations-on-service}

#### GET list of containers

A `GET` issued to the endpoint root returns a list of buckets associated with the requesting account.

**Syntax**

~~~
GET http://{endpoint}/
~~~

**Sample Request:**

~~~
GET / HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Host: s3-api.us-geo.objectstorage.softlayer.net
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

A `PUT` issued to the endpoint root will create a bucket when a string is provided.

**Syntax**

~~~
PUT http://{endpoint}/{bucket-name}
~~~

**Sample Request:**

This is an example of creating a new bucket called 'apiary'.

~~~
PUT /apiary HTTP/1.1
Content-Type: text/html
Host: s3-api.us-geo.objectstorage.softlayer.net
X-Amz-Date: 20160821T052842Z
Authorization:{authorization-string}
~~~

**Sample Response:**

~~~
HTTP/1.1 200 OK
Date: Wed, 24 Aug 2016 17:45:25 GMT
X-Clv-Request-Id: dca204eb-72b5-4e2a-a142-808d2a5c2a87
Accept-Ranges: bytes
Server: Cleversafe/3.9.0.115
X-Clv-S3-Version: 2.5
x-amz-request-id: dca204eb-72b5-4e2a-a142-808d2a5c2a87
Content-Length: 0
~~~

#### GET Bucket (list objects)

When a `GET` request is given to a specific container, a list of the contents are returned.  This listing is limited to the first 1,000 objects.

**Syntax**

~~~
GET http://{endpoint}/{bucket-name}
~~~

**Sample Request**

This requests lists the objects inside the "raw-photos" bucket.

~~~
GET /raw-photos HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Host: s3-api.us-geo.objectstorage.softlayer.net
X-Amz-Date: 20160822T225156Z
Authorization: {authorization-string}
~~~

**Sample Response**

~~~
HTTP/1.1 200 OK
Date: Wed, 24 Aug 2016 17:36:24 GMT
X-Clv-Request-Id: 9f39ff2e-55d1-461b-a6f1-2d0b75138861
Accept-Ranges: bytes
Server: Cleversafe/3.9.0.115
X-Clv-S3-Version: 2.5
x-amz-request-id: 9f39ff2e-55d1-461b-a6f1-2d0b75138861
Content-Type: application/xml
Content-Length: 909
~~~
~~~
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<ListBucketResult xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
    <Name>apiary</Name>
    <Prefix></Prefix>
    <Marker></Marker>
    <MaxKeys>1000</MaxKeys>
    <Delimiter></Delimiter>
    <IsTruncated>false</IsTruncated>
    <Contents>
        <Key>photo_1</Key>
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
        <Key>photo_2</Key>
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

#### DELETE Bucket

A `DELETE` issued to an empty bucket deletes the bucket. *Only empty buckets can be deleted.*

**Syntax**

~~~
DELETE http://{endpoint}/{bucket-name}
~~~

**Sample Request**

~~~
DELETE /apiary HTTP/1.1
Content-Type: text/html
Host: s3-api.us-geo.objectstorage.softlayer.net
X-Amz-Date: 20160822T064812Z
Authorization: {authorization-string}
~~~

The server responds with `204 No Content`.

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
Accept-Ranges: bytes
Content-Length: 12
Content-Type: application/octet-stream
Date: Mon, 22 Aug 2016 23:02:50 GMT
ETag: "3c66102af4bdeb4dc7cc08b6d800b82b"
Last-Modified: Thu, 18 Aug 2016 16:27:32 GMT
Server: Cleversafe/3.9.0.115
X-Clv-Request-Id: f11f3447-cfab-476b-b5da-fa7219d4c7bd
X-Clv-S3-Version: 2.5
x-amz-request-id: f11f3447-cfab-476b-b5da-fa7219d4c7bd
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

