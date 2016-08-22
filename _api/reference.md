---
layout: page
title:  "Reference & Examples"
featured: true
weight: 3
tags: []
author: Nick
dateAdded: January 14th, 2016
---

### Table of Contents

* [Overview](#overview)
* [Common Headers and Error Responses](#headers-and-error-response)
* [Operations on Buckets](#operations-on-service)
* [Operations on Objects](#operations-on-objects)

###  Overview
{: #overview}



The IBM Cloud Object Storage (COS) Application Programming Interface (API) enables application developers to use existing Amazon Simple Storage Service (S3) applications to access object buckets. 

COS supports the most commonly used subset of Amazon S3 API operations. This document details the subset of the S3 API that COS API supports. Any undocumented methods are unsupported.

### Prerequisites
{: #prerequisite}
Ensure you have an account with sufficient permissions.

#### Common Headers and Error Responses
{: #headers-and-error-response} 

#### Common Request Headers
The following are the common request headers used in S3, and supported by COS. Unsupported headers will be ignored if sent in a request.

| Header             | Supported  |  Note                               |
|--------------------|------------|-------------------------------------|
| Authorization      | Yes        | AWS, AWS4, or BASIC authentication. |
| Content-Length     | Yes        | Chunked encoding also supported.    |
| Content-Type       | Yes        | Stored as object metadata.          |
| Content-MD5        | Yes        |                                     |
| Date               | Required   |                                     |
| Expect             | Yes        |                                     | 
| Host               | Supported  |                                     |
|x-amz-content-sha256| Yes        | Used with signature version 4 authenticated requests. |
| x-amz-date         | Yes        |                                     |
| User-Agent         | Ignored    |                                     |
|x-amz-security-token| Ignored    | Tokens are not supported.           |   


####  Common Response Headers
The following are the common response headers used in S3 supported by COS Dedicated IBM Managed. 

|  Header        | Supported | Note |
|----------------|-----------|------|
| Content-Length | Yes       |      |
|Connection     | Yes       |      | 
| Date           | Yes       |      |
| ETag           | Yes       | Contains MD5 checksum. |
| Server         | Yes       |      | 
|x-amz-request-id| Yes       |      |
|X-Clv-Request-Id|           | Unique identifier per response a COS Dedicated IBM Managed Support Engineer uses for diagnostics and troubleshooting purposes. |
|x-amz-version-id| Yes       |      |
|X-Clv-S3-Version|           | S3 version the COS Dedicated IBM Managed API used for the request. |

#### Error Responses

In addition, the following COS Dedicated IBM Managed unique response code might be used.

| Error Code  | Description  | HTTP Status Code  |
|-------------|--------------|-------------------|
| VaultQuotaExceeded | The capacity used on the target bucket has exceeded a hard quota |507 |




### Operations on Buckets

#### GET Bucket

All parameters, operation-specific request and response headers and the response body conform to the S3 API under normal operations when the name index is enabled.

When Recovery Listing is enabled, the results of a ``GET`` Bucket (List Objects) operation are returned in non-lexicographical order. Pagination is supported using the normal convention by utilizing the marker parameter and ``nextMarker`` response element. A search using the prefix and delimiter parameters in recovery listing mode will result in an HTTP ``405 Method Not Allowed`` response code.
If the name index is disabled for a bucket and recovery mode listing is not enabled, all listing operation requests also will result in an HTTP ``405 Method Not Allowed`` response code.

COS Dedicated IBM Managed ignores any value greater than 1,000 for ``max-keys`` and returns only up to 1,000 keys.

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

#### PUT Bucket
**Sample Request:**

~~~
PUT /{bucket-name} HTTP/1.1
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW
Host: s3-api.{endpoint}.objectstorage.softlayer.net
X-Amz-Date: 20160821T052842Z
Authorization:{authorization-string}
~~~

### Operations on Objects
{: #operations-on-objects}
The following table describes which object operations are supported in the COS Dedicated IBM Managed API. It describes all operations on objects available as part of the S3 API. Versioning is supported for ``DELETE``, ``GET`` and ``PUT`` where the user can provide a version ID to delete and get a specific version.

**Object Operations supported by COS Dedicated IBM Managed:**

| Operation | Supported | Note |
| DELETE Object | Yes | |
| DELETE Multiple Objects | Yes | |
| GET Object | Yes | |
| GET Object ACL | Yes | |
| GET Object torrent | No | |
| HEAD Object | Yes | |
| OPTIONS Object | Yes | |
| POST Object | Yes | |
| POST Object restore | No | |
| PUT Object | Yes | |
| PUT Object ACL | Yes | |
| PUT Object (Copy) | Yes | Copying objects results in a full-Width read and then a full-Width write. These full copies will result in high bandwidth utilization within the system until the copy is complete. |
| Initiate Multipart Upload | Yes | |
| Upload Part | Yes |
| Upload Part (Copy) | Yes | |
| Complete Multipart Upload | Yes | |
| Abort Multipart Upload | Yes | |
| List Parts | Yes | |

#### GET Object
Unless otherwise noted in the following table, all parameters, operation-specific request and response headers and the response body conform to the S3 API.

**GET object support with COS Dedicated IBM Managed:**

| Operation | Supported |
|-----------|-----------|
| Request Parameters | Yes |
| Request Headers | Yes |
| Response Headers | Yes |

#### HEAD Object

Unless otherwise noted in the following table, all parameters, operation-specific request and response headers and the response body conform to the S3 API.

**GET object support with COS Dedicated IBM Managed:**

| Operation | Supported |
|-----------|-----------|
| Request Parameters | Yes |
| Request Headers | Yes |
| Response Headers | Yes |

#### POST Object

Unless otherwise noted in the following tables, all parameters, operation specific request and response headers and the response body conform to the S3 API.

**POST object support with COS Dedicated IBM Managed - Form Fields:**

| Form Field | Supported |
|------------|-----------|
| AWSAccessKeyId | Yes   | 
| acl  | Yes |
| Cache-Control | Ignored |
| Content-Type | Ignored |
| Content-Disposition  | Ignored |
| Content-Encoding  | Ignored |
| Expires | Ignored |
| key | Yes |
| policy | Yes |
| success_action_redirect, redirect | Ignored |
| success_action_status | Yes |
| signature | Yes |
| x-amz-security-token | Ignored |
| Other field names prefixed with x-amz-meta- | Yes |
| file | Yes |

**POST object support with COS Dedicated IBM Managed - Response Headers:**

| Response Header | Supported |
|-----------------|-----------|
| X-Amz-Expiration | No |
| X-Amz-Server-Side-Encryption | No |

#### DELETE Object

Unless otherwise noted in the following table, all parameters, operation specific request and response headers and the response body conform to the S3 API.

**DELETE object support with COS Dedicated IBM Managed:**

| Operation | Header | Supported |
|-----------|--------|-----------|
| Request Headers | x-amz-mfa | Ignored |
| Response Headers | x-amz-delete-marker | Yes |




#### PUT Object
Unless otherwise noted in in the following table, all parameters, operation specific request and response headers and the response body conform to the S3 API.
