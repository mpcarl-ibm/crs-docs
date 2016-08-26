---
layout: page
title:  "API Reference & Examples"
featured: true
weight: 3
tags: []
author: Nick
dateAdded: August 18th, 2016
---

### Table of Contents

* [Overview](#overview)
* [Common Headers and Error Responses](#headers-and-error-response)
* [Operations on Service](#operations-on-service)
* [Operations on Buckets](#operations-on-buckets)
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
| Authorization      | **Required** for all requests. AWS4 authentication  |
| Host               | **Required** for all requests.                 |
| x-amz-date         | **Required** for all requests.                 |
|x-amz-content-sha256| **Required** for uploading objects. |
| Content-Length     | **Required** for uploading objects, chunked encoding also supported.    |
| Content-MD5        |                                  |
| Expect             |                                    | 



####  Common Response Headers
The following table describes common response headers.


|  Header        | Note |
|----------------|------|
| Content-Length |      |
|Connection     |       | 
| Date           |       |
| Server         |      | 
|x-amz-request-id|       |
|X-Clv-Request-Id|  Unique identifier per response an IBM COS Support Engineer uses for diagnostics and troubleshooting purposes. |
|x-amz-version-id|      |

### Operations on Service
{: #operations-on-service}

#### List buckets belonging to an account

A `GET` issued to the endpoint root returns a list of buckets associated with the requesting account.

**Syntax**

{% highlight bash %}
GET http://{endpoint}/
{% endhighlight %}

**Sample Request:**

{% highlight http %}
GET / HTTP/1.1
Content-Type: text/plain
Host: s3-api.us-geo.objectstorage.softlayer.net
X-Amz-Date: 20160822T030815Z
Authorization: {authorization-string}
{% endhighlight %}

**Sample Response:**

{% highlight xml %}
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
{% endhighlight %}

### Operations on Buckets
{: #operations-on-buckets}

#### Create a new bucket

A `PUT` issued to the endpoint root will create a bucket when a string is provided.  Bucket names must be unique, and accounts are limited to 100 buckets each.  Bucket names must be DNS-compliant (between 3 and 63 characters long, can not be formatted as an IP address, and can not begin or end with a period). 

**Syntax**

{% highlight bash %}
PUT http://{endpoint}/{bucket-name}
{% endhighlight %}

**Sample Request:**

This is an example of creating a new bucket called 'images'.

{% highlight http %}
PUT /images HTTP/1.1
Content-Type: text/plain
Host: s3-api.us-geo.objectstorage.softlayer.net
X-Amz-Date: 20160821T052842Z
Authorization:{authorization-string}
{% endhighlight %}

**Sample Response:**

{% highlight http %}
HTTP/1.1 200 OK
Date: Wed, 24 Aug 2016 17:45:25 GMT
X-Clv-Request-Id: dca204eb-72b5-4e2a-a142-808d2a5c2a87
Accept-Ranges: bytes
Server: Cleversafe/3.9.0.115
X-Clv-S3-Version: 2.5
x-amz-request-id: dca204eb-72b5-4e2a-a142-808d2a5c2a87
Content-Length: 0
{% endhighlight %}

#### List objects in a given bucket

When a `GET` request is given to a specific container, a list of the contents are returned.  This listing is limited to the first 1,000 objects.

**Syntax**

{% highlight bash %}
GET http://{endpoint}/{bucket-name}
{% endhighlight %}

**Sample Request**

This requests lists the objects inside the "apiary" bucket.

{% highlight http %}
GET /apiary HTTP/1.1
Content-Type: text/plain
Host: s3-api.us-geo.objectstorage.softlayer.net
X-Amz-Date: 20160822T225156Z
Authorization: {authorization-string}
{% endhighlight %}

**Sample Response**

{% highlight http %}
HTTP/1.1 200 OK
Date: Wed, 24 Aug 2016 17:36:24 GMT
X-Clv-Request-Id: 9f39ff2e-55d1-461b-a6f1-2d0b75138861
Accept-Ranges: bytes
Server: Cleversafe/3.9.0.115
X-Clv-S3-Version: 2.5
x-amz-request-id: 9f39ff2e-55d1-461b-a6f1-2d0b75138861
Content-Type: application/xml
Content-Length: 909
{% endhighlight %}
{% highlight xml %}
<ListBucketResult xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
  <Name>apiary</Name>
  <Prefix/>
  <Marker/>
  <MaxKeys>1000</MaxKeys>
  <Delimiter/>
  <IsTruncated>false</IsTruncated>
  <Contents>
    <Key>drone-bee</Key>
    <LastModified>2016-08-25T17:38:38.549Z</LastModified>
    <ETag>"0cbc6611f5540bd0809a388dc95a615b"</ETag>
    <Size>4</Size>
    <Owner>
      <ID>7dd4f57d38484131999e6739e40279a7</ID>
      <DisplayName>7dd4f57d38484131999e6739e40279a7</DisplayName>
    </Owner>
    <StorageClass>STANDARD</StorageClass>
  </Contents>
  <Contents>
    <Key>soldier-bee</Key>
    <LastModified>2016-08-25T17:49:06.006Z</LastModified>
    <ETag>"37d4c94839ee181a2224d6242176c4b5"</ETag>
    <Size>11</Size>
    <Owner>
      <ID>7dd4f57d38484131999e6739e40279a7</ID>
      <DisplayName>7dd4f57d38484131999e6739e40279a7</DisplayName>
    </Owner>
    <StorageClass>STANDARD</StorageClass>
  </Contents>
  <Contents>
    <Key>worker-bee</Key>
    <LastModified>2016-08-25T17:46:53.288Z</LastModified>
    <ETag>"d34d8aada2996fc42e6948b926513907"</ETag>
    <Size>467</Size>
    <Owner>
      <ID>7dd4f57d38484131999e6739e40279a7</ID>
      <DisplayName>7dd4f57d38484131999e6739e40279a7</DisplayName>
    </Owner>
    <StorageClass>STANDARD</StorageClass>
  </Contents>
</ListBucketResult>
{% endhighlight %}

#### Delete a bucket

A `DELETE` issued to an empty bucket deletes the bucket. *Only empty buckets can be deleted.*

**Syntax**

{% highlight bash %}
DELETE http://{endpoint}/{bucket-name}
{% endhighlight %}

**Sample Request**

{% highlight http %}
DELETE /images HTTP/1.1
Host: s3-api.us-geo.objectstorage.softlayer.net
X-Amz-Date: 20160822T064812Z
Authorization: {authorization-string}
{% endhighlight %}

The server responds with `204 No Content`.

If a non-empty bucket is requested for deletion, the server responds with `409 Conflict`.

**Sample Request**

{% highlight http %}
DELETE /apiary HTTP/1.1
Authorization: {authorization-string}
x-amz-date: 20160825T174049Z
Host: s3-api.us-geo.objectstorage.softlayer.net
{% endhighlight %}

**Sample Response**

{% highlight xml %}
<Error>
  <Code>BucketNotEmpty</Code>
  <Message>The bucket you tried to delete is not empty.</Message>
  <Resource>/apiary/</Resource>
  <RequestId>9d2bbc00-2827-4210-b40a-8107863f4386</RequestId>
  <httpStatusCode>409</httpStatusCode>
</Error>
{% endhighlight %}

### Operations on Objects
{: #operations-on-objects}

#### Upload an object

A `PUT` given a path to an object uploads an object. A SHA256 hash of the object is a required header.


**Syntax**

{% highlight bash %}
PUT http://{endpoint}/{bucket-name}/{object-name}
{% endhighlight %}

**Sample Request**

{% highlight http %}
PUT /apiary/queen-bee HTTP/1.1
Authorization: {authorization-string}
x-amz-date: 20160825T183001Z
x-amz-content-sha256: 309721641329cf441f3fa16ef996cf24a2505f91be3e752ac9411688e3435429
Content-Type: text/plain; charset=utf-8
Host: s3-api.us-geo.objectstorage.softlayer.net
Connection: close
Content-Length: 533

 The 'queen' bee is developed from larvae selected by worker bees and fed a 
 substance referred to as 'royal jelly' to accelerate sexual maturity. After a 
 short while the 'queen' is the mother of nearly every bee in the hive, and 
 the colony will fight fiercly to protect her. 

{% endhighlight %}

**Sample Response**

{% highlight http %}
HTTP/1.1 200 OK
Date: Thu, 25 Aug 2016 18:30:02 GMT
X-Clv-Request-Id: 9f0ca49a-ae13-4d2d-925b-117b157cf5c3
Accept-Ranges: bytes
Server: Cleversafe/3.9.0.121
X-Clv-S3-Version: 2.5
x-amz-request-id: 9f0ca49a-ae13-4d2d-925b-117b157cf5c3
ETag: "3ca744fa96cb95e92081708887f63de5"
Content-Length: 0
{% endhighlight %}

#### Get an objects headers

A `HEAD` given a path to an object retrieves that object's headers.

**Syntax**

{% highlight bash %}
HEAD http://{endpoint}/{bucket-name}/{object-name}
{% endhighlight %}

**Sample Request**

{% highlight http %}
HEAD /apiary/soldier-bee HTTP/1.1
Authorization: {authorization-string}
x-amz-date: 20160825T183244Z
Host: s3-api.sjc-us-geo.objectstorage.softlayer.net
Connection: close
{% endhighlight %}

**Sample Response**

{% highlight http %}
HTTP/1.1 200 OK
Date: Thu, 25 Aug 2016 18:32:44 GMT
X-Clv-Request-Id: da214d69-1999-4461-a130-81ba33c484a6
Accept-Ranges: bytes
Server: Cleversafe/3.9.0.121
X-Clv-S3-Version: 2.5
x-amz-request-id: da214d69-1999-4461-a130-81ba33c484a6
ETag: "37d4c94839ee181a2224d6242176c4b5"
Content-Type: text/plain; charset=UTF-8
Last-Modified: Thu, 25 Aug 2016 17:49:06 GMT
Content-Length: 11
{% endhighlight %}

#### Download an object

A `GET` given a path to an object uploads an object.

**Syntax**

{% highlight bash %}
GET http://{endpoint}/{bucket-name}/{object-name}
{% endhighlight %}

**Sample Request**

{% highlight http %}
GET /apiary/worker-bee HTTP/1.1
Authorization: {authorization-string}
Host: s3-api.us-geo.objectstorage.softlayer.net
Connection: close
{% endhighlight %}

**Sample Response**

{% highlight http %}
HTTP/1.1 200 OK
Date: Thu, 25 Aug 2016 18:34:25 GMT
X-Clv-Request-Id: 116dcd6b-215d-4a81-bd30-30291fa38f93
Accept-Ranges: bytes
Server: Cleversafe/3.9.0.121
X-Clv-S3-Version: 2.5
x-amz-request-id: 116dcd6b-215d-4a81-bd30-30291fa38f93
ETag: "d34d8aada2996fc42e6948b926513907"
Content-Type: text/plain; charset=UTF-8
Last-Modified: Thu, 25 Aug 2016 17:46:53 GMT
Content-Length: 467

 Female bees that are not fortunate enough to be selected to be the 'queen'
 while they were still larvae become known as 'worker' bees. These bees lack 
 the ability to reproduce and instead ensure that the hive functions smoothly, 
 acting almost as a single organism in fulfilling their purpose.
{% endhighlight %}

#### DELETE Object

#### Download an object

A `DELETE` given a path to an object deletes an object.

**Syntax**

{% highlight bash %}
DELETE http://{endpoint}/{bucket-name}/{object-name}
{% endhighlight %}

**Sample Request**

{% highlight http %}
DELETE /apiary/soldier-bee HTTP/1.1
Authorization: {authorization-string}
Host: s3-api.sjc-us-geo.objectstorage.softlayer.net
Connection: close
{% endhighlight %}

**Sample Response**

{% highlight http %}
HTTP/1.1 204 No Content
Date: Thu, 25 Aug 2016 17:44:57 GMT
X-Clv-Request-Id: 8ff4dc32-a6f0-447f-86cf-427b564d5855
Accept-Ranges: bytes
Server: Cleversafe/3.9.0.121
X-Clv-S3-Version: 2.5
x-amz-request-id: 8ff4dc32-a6f0-447f-86cf-427b564d5855
{% endhighlight %}


