---
layout: page
title:  "API reference and examples"
featured: true
weight: 2
tags: []
author: Nick
dateAdded: August 18th, 2016
---

> IBM COS Standard Cross Region is currently in open trial.  Please visit [IBM Cloud](https://www.softlayer.com/Store/orderService/objectStorage) to participate.

### Table of Contents

* [Overview](#overview)
* [Common Headers and Error Responses](#headers-and-error-response)
* [Operations on Service](#operations-on-service)
* [Operations on Buckets](#operations-on-buckets)
* [Operations on Objects](#operations-on-objects)

###  Overview
{: #overview}

The IBM Cloud Object Storage implementation of the S3 API supports the most commonly used subset of Amazon S3 API operations. A complete list of supported operations can be found [here]({{ site.baseurl }}/beta/api/overview/).

> **Note:** this documentation for the open trial is in-progress and does not yet provide detailed usage guidance and examples for all supported operations. 

### Authorization
The `authorization` header is required for all requests, and is calculated according to the AWS Signature Version 4 protocol.  This provides identity verification and in-transit data integrity, and is tied to the timestamp of the request.  The header is composed of four components: an algorithm declaration, credential information, signed headers, and the calculated signature:  

```
AWS4-HMAC-SHA256 
Credential={access-key}/{date}/{region}/s3/aws4_request, 
SignedHeaders=host;x-amz-date;{other-required-headers}, 
Signature={signature}
```

The date is provided in `YYYYMMDD` format, and the region can be any value (e.g. `us-standard`). The `host` and `x-amz-date` headers are always required, and depending on the request other headers may be required as well (e.g. `x-amz-content-sha256`).  The 256 bit signature is given as 64 hexadecimal character string and is calculated from multiple combinations and hashes of the request elements.  Due to the need to recalulate the signature for every individual request, many developers prefer to use a tool or SDK that will produce the authorization header automatically.

> **INTERNAL NOTE**: The signature calculation process is fairly involved, and will be illustracted here but is still in-progress.  [AWS signature calculation documentation can be viewed here](http://docs.aws.amazon.com/AmazonS3/latest/API/sig-v4-header-based-auth.html). 


### Common Headers and Error Responses
{: #headers-and-error-response} 

#### Common Request Headers
The following table describes supported common request headers. Headers not listed here will be ignored if sent in a request.

| Header             | Note                               |
|--------------------|-------------------------------------|
| Authorization      | **Required** for all requests (AWS Signature Version 4).   |
| Host               | **Required** for all requests.                 |
| x-amz-date         | **Required** for all requests.                 |
|x-amz-content-sha256| **Required** for uploading objects or any request with information in the body. |
| Content-Length     | **Required** for uploading objects, chunked encoding also supported.    |
| Content-MD5        | A 128-bit MD5 hash value of the message being sent.                  |
| Expect             | `100-continue` waits for the headers to be accepted before sending the body.  | 

####  Common Response Headers
The following table describes common response headers.


|  Header        | Note |
|----------------|------|
| Content-Length | The length of the request body in bytes.      |
|Connection     |  Indicates whether the connection is open or closed.     | 
| Date           | Timestamp of the request.     |
| Server         | Name of the responding server.     | 
|X-Clv-Request-Id|  Unique identifier generated per request. |

### Operations on Service
{: #operations-on-service}

#### List buckets belonging to an account

A `GET` issued to the endpoint root returns a list of buckets associated with the requesting account.

**Syntax**

```ShellSession
GET http://{endpoint}/
```

**Sample Request:**

```http
GET / HTTP/1.1
Content-Type: text/plain
Host: s3-api.us-geo.objectstorage.softlayer.net
X-Amz-Date: 20160822T030815Z
Authorization: {authorization-string}
```

**Sample Response:**

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<ListAllMyBucketsResult xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
    <Owner>
        <ID>{account-id}</ID>
        <DisplayName>{account-id}</DisplayName>
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
```

### Operations on Buckets
{: #operations-on-buckets}

#### Create a new bucket

A `PUT` issued to the endpoint root will create a bucket when a string is provided.  Bucket names must be unique, and accounts are limited to 100 buckets each.  Bucket names must be DNS-compliant; names between 3 and 63 characters long must be made of lowercase letters, numbers, and dashes. Bucket names must begin and end with a lowercase letter or number.  Using periods as a separator within bucket names is not recommended, and bucket names resembling IP addresses are not allowed.

**Syntax**

```bash
PUT http://{endpoint}/{bucket-name}
```

**Sample Request:**

This is an example of creating a new bucket called 'images'.

```http
PUT /images HTTP/1.1
Content-Type: text/plain
Host: s3-api.us-geo.objectstorage.softlayer.net
X-Amz-Date: 20160821T052842Z
Authorization:{authorization-string}
```

**Sample Response:**

```http
HTTP/1.1 200 OK
Date: Wed, 24 Aug 2016 17:45:25 GMT
X-Clv-Request-Id: dca204eb-72b5-4e2a-a142-808d2a5c2a87
Accept-Ranges: bytes
Server: Cleversafe/3.9.0.115
X-Clv-S3-Version: 2.5
x-amz-request-id: dca204eb-72b5-4e2a-a142-808d2a5c2a87
Content-Length: 0
```

#### List objects in a given bucket

When a `GET` request is given to a specific container, a list of the contents are returned.  This listing is limited to the first 1,000 objects.

**Syntax**

```bash
GET http://{endpoint}/{bucket-name}
```

**Sample Request**

This requests lists the objects inside the "apiary" bucket.

```http
GET /apiary HTTP/1.1
Content-Type: text/plain
Host: s3-api.us-geo.objectstorage.softlayer.net
X-Amz-Date: 20160822T225156Z
Authorization: {authorization-string}
```

**Sample Response**

```http
HTTP/1.1 200 OK
Date: Wed, 24 Aug 2016 17:36:24 GMT
X-Clv-Request-Id: 9f39ff2e-55d1-461b-a6f1-2d0b75138861
Accept-Ranges: bytes
Server: Cleversafe/3.9.0.115
X-Clv-S3-Version: 2.5
x-amz-request-id: 9f39ff2e-55d1-461b-a6f1-2d0b75138861
Content-Type: application/xml
Content-Length: 909
```
```xml
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
      <ID>{account-id}</ID>
      <DisplayName>{account-id}</DisplayName>
    </Owner>
    <StorageClass>STANDARD</StorageClass>
  </Contents>
  <Contents>
    <Key>soldier-bee</Key>
    <LastModified>2016-08-25T17:49:06.006Z</LastModified>
    <ETag>"37d4c94839ee181a2224d6242176c4b5"</ETag>
    <Size>11</Size>
    <Owner>
      <ID>{account-id}</ID>
      <DisplayName>{account-id}</DisplayName>
    </Owner>
    <StorageClass>STANDARD</StorageClass>
  </Contents>
  <Contents>
    <Key>worker-bee</Key>
    <LastModified>2016-08-25T17:46:53.288Z</LastModified>
    <ETag>"d34d8aada2996fc42e6948b926513907"</ETag>
    <Size>467</Size>
    <Owner>
      <ID>{account-id}</ID>
      <DisplayName>{account-id}</DisplayName>
    </Owner>
    <StorageClass>STANDARD</StorageClass>
  </Contents>
</ListBucketResult>
```

#### Delete a bucket

A `DELETE` issued to an empty bucket deletes the bucket. *Only empty buckets can be deleted.*

**Syntax**

```bash
DELETE http://{endpoint}/{bucket-name}
```

**Sample Request**

```http
DELETE /images HTTP/1.1
Host: s3-api.us-geo.objectstorage.softlayer.net
x-amz-date: 20160822T064812Z
Authorization: {authorization-string}
```

The server responds with `204 No Content`.

If a non-empty bucket is requested for deletion, the server responds with `409 Conflict`.

**Sample Request**

```http
DELETE /apiary HTTP/1.1
Authorization: {authorization-string}
x-amz-date: 20160825T174049Z
Host: s3-api.us-geo.objectstorage.softlayer.net
```

**Sample Response**

```xml
<Error>
  <Code>BucketNotEmpty</Code>
  <Message>The bucket you tried to delete is not empty.</Message>
  <Resource>/apiary/</Resource>
  <RequestId>9d2bbc00-2827-4210-b40a-8107863f4386</RequestId>
  <httpStatusCode>409</httpStatusCode>
</Error>
```

#### Create an access control list for a bucket

A `PUT` issued to a bucket with the proper parameters creates an access control list (ACL) for that bucket.  Access control lists allow for granting different sets of permissions to different storage accounts using the account's UUID, or by using a pre-made ACL.

> **INTERNAL NOTE**: At this time credentials are generated for each storage account, not for indvidual users.  As such, ACLs do not have the ability to restrict or grant access to a given user, only to a storage account. However, `public-read-write` allows any other CRS storage account to access the resource, as well as the general public. 

ACLs can use pre-made permissions sets (or 'canned ACLs') or be customized in the body of the request. Pre-made ACLs are specified using the `x-amz-acl` header with `private`, `public-read`, or `public-read-write` as the value. Custom ACLs are specified using XML in the request body and can grant `READ`, `WRITE`, `READ_ACP` (read ACL), `WRITE_ACP` (write ACL), or `FULL_CONTROL` permissions to a given storage account.

**Syntax**

```bash
PUT http://{endpoint}/{bucket-name}?acl=
```

**Sample Request:** Basic pre-made ACL

This is an example of specifying a pre-made ACL to allow for `public-read` access to the "apiary" bucket. This allows any storage account to view the bucket's contents and ACL, and to access objects.

```http
PUT /apiary?acl= HTTP/1.1
Authorization: {authorization-string}
x-amz-date: 20161011T190354Z
x-amz-acl: public-read
Host: s3-api.us-geo.objectstorage.softlayer.net
```

**Sample Response:**

```http
HTTP/1.1 200 OK
Date: Tue, 4 Oct 2016 19:03:55 GMT
X-Clv-Request-Id: 73d3cd4a-ff1d-4ac9-b9bb-43529b11356a
Accept-Ranges: bytes
Server: Cleversafe/3.9.0.129
X-Clv-S3-Version: 2.5
x-amz-request-id: 73d3cd4a-ff1d-4ac9-b9bb-43529b11356a
Content-Length: 0
```

**Sample Request:** Custom ACL

This is an example of specifying a custom ACL to allow for another account to view the ACL for the "apiary" bucket, but not to view or access objects stored inside the bucket. Additionally, a third account is given full access to the same bucket as another element of the same ACL.

```http
PUT /apiary?acl= HTTP/1.1
Authorization: {authorization-string}
x-amz-date: 20161011T190354Z
Host: s3-api.us-geo.objectstorage.softlayer.net
```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<AccessControlPolicy xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
  <Owner>
    <ID>{owner-storage-account-uuid}</ID>
    <DisplayName>OwnerDisplayName</DisplayName>
  </Owner>
  <AccessControlList>
    <Grant>
      <Grantee xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="CanonicalUser">
        <ID>{first-grantee-storage-account-uuid}</ID>
        <DisplayName>Grantee1DisplayName</DisplayName>
      </Grantee>
      <Permission>READ_ACP</Permission>
    </Grant>
    <Grant>
      <Grantee xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="CanonicalUser">
        <ID>{second-grantee-storage-account-uuid}</ID>
        <DisplayName>Grantee2DisplayName</DisplayName>
      </Grantee>
      <Permission>FULL_CONTROL</Permission>
    </Grant>
  </AccessControlList>
</AccessControlPolicy>
```

**Sample Response:**

```http
HTTP/1.1 200 OK
Date: Tue, 4 Oct 2016 19:03:55 GMT
X-Clv-Request-Id: 73d3cd4a-ff1d-4ac9-b9bb-43529b11356a
Accept-Ranges: bytes
Server: Cleversafe/3.9.0.129
X-Clv-S3-Version: 2.5
x-amz-request-id: 73d3cd4a-ff1d-4ac9-b9bb-43529b11356a
```

#### Retrieve the access control list for a bucket

A `GET` issued to a bucket with the proper parameters retrieves the ACL for a bucket.

**Syntax**

```bash
XXX http://{endpoint}/{bucket-name}?acl=
```

**Sample Request:**

This is an example of retrieving a bucket ACL.

```http
GET /apiary?acl= HTTP/1.1
Authorization: {authorization-string}
x-amz-date: 20161011T190354Z
Host: s3-api.us-geo.objectstorage.softlayer.net
```

**Sample Response:**

```http
HTTP/1.1 200 OK
Date: Wed, 5 Oct 2016 14:14:34 GMT
X-Clv-Request-Id: eb57e60e-d84e-4237-b18a-be9c2bb0deb8
Accept-Ranges: bytes
Server: Cleversafe/3.9.0.129
X-Clv-S3-Version: 2.5
x-amz-request-id: eb57e60e-d84e-4237-b18a-be9c2bb0deb8
Content-Type: application/xml
Content-Length: 550
```

```xml
<AccessControlPolicy xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
  <Owner>
    <ID>{owner-storage-account-uuid}</ID>
    <DisplayName>{owner-storage-account-uuid}</DisplayName>
  </Owner>
  <AccessControlList>
    <Grant>
      <Grantee xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="CanonicalUser">
        <ID>{owner-storage-account-uuid}</ID>
        <DisplayName>{owner-storage-account-uuid}</DisplayName>
      </Grantee>
      <Permission>FULL_CONTROL</Permission>
    </Grant>
  </AccessControlList>
</AccessControlPolicy>
```

#### List canceled/incomplete multipart uploads for a bucket

A `GET` issued to a bucket with the proper parameters retrieves information about any canceled or incomplete multi-part uploads for a bucket.

**Syntax**

```bash
GET http://{endpoint}/{bucket-name}?uploads=
```

**Parameters**

Name | Type | Description
--- | ---- | ------------
`prefix` | string | Constrains reponse to object names beginning with `{prefix}`.
`delimiter` | string | Groups objects within between the `prefix` and the `delimiter`.
`encoding-type` | string | If unicode characters that are not supported by XML are used in an object name, this parameter can be set to `url` to properly encode the reponse.
`max-uploads` | integer | Restricts the number of objects to display in the response.  Default and maximum is 1,000.
`key-marker` | string | Specifies the start of the which parts should be listed.
`upload-id-marker` | string | Ignored if `key-marker` is not specified, otherwise sets a point at which to begin listing parts above `upload-id-marker`.
 
**Sample Request:**

This is an example of retrieving all current cancelled and incomplete multipart uploads.

```http
GET /apiary?uploads= HTTP/1.1
Authorization: {authorization-string}
x-amz-date: 20161011T190354Z
Host: s3-api.us-geo.objectstorage.softlayer.net
```

**Sample Response:** No multipart uploads in progress.

```http
HTTP/1.1 200 OK
Date: Wed, 5 Oct 2016 15:22:27 GMT
X-Clv-Request-Id: 9fa96daa-9f37-42ee-ab79-0bcda049c671
Accept-Ranges: bytes
Server: Cleversafe/3.9.0.129
X-Clv-S3-Version: 2.5
x-amz-request-id: 9fa96daa-9f37-42ee-ab79-0bcda049c671
Content-Type: application/xml
Content-Length: 374
```

```xml
<ListMultipartUploadsResult xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
  <Bucket>bucket-1</Bucket>
  <KeyMarker/>
  <UploadIdMarker/>
  <NextKeyMarker/>
  <NextUploadIdMarker/>
  <MaxUploads>1000</MaxUploads>
  <IsTruncated>false</IsTruncated>
</ListMultipartUploadsResult>
```

#### List any cross-origin resource sharing configuration for a bucket

A `GET` issued to a bucket with the proper parameters retrieves information about cross-origin resource sharing (CORS) configuration for a bucket.

**Syntax**

```bash
GET http://{endpoint}/{bucket-name}?cors=
```

**Sample Request:**

This is an example of listing a CORS configuration on the "apiary" bucket.

```http
GET /apiary?cors= HTTP/1.1
Authorization: {authorization-string}
x-amz-date: 20161011T190354Z
Host: s3-api.us-geo.objectstorage.softlayer.net
```

**Sample Response:** No CORS configuration set

```http
HTTP/1.1 200 OK
Date: Wed, 5 Oct 2016 15:20:30 GMT
X-Clv-Request-Id: 0b69bce1-8420-4f93-a04a-35d7542799e6
Accept-Ranges: bytes
Server: Cleversafe/3.9.0.129
X-Clv-S3-Version: 2.5
x-amz-request-id: 0b69bce1-8420-4f93-a04a-35d7542799e6
Content-Type: application/xml
Content-Length: 123
```

```xml
<CORSConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/"/>
```

#### Create a cross-origin resource sharing configuration for a bucket

A `PUT` issued to a bucket with the proper parameters creates or replaces a cross-origin resource sharing (CORS) configuration for a bucket.

**Syntax**

```bash
PUT http://{endpoint}/{bucket-name}?cors=
```

**Sample Request:**

This is an example of adding a CORS configuration that allows requests from `www.ibm.com` to issue `GET`, `PUT`, and `POST` requests to the bucket.

```http
GET /apiary?cors= HTTP/1.1
Authorization: {authorization-string}
x-amz-date: 20161011T190354Z
x-amz-content-sha256: 2938f51643d63c864fdbea618fe71b13579570a86f39da2837c922bae68d72df
Content-MD5: GQmpTNpruOyK6YrxHnpj7g==
Content-Type: text/plain
Host: s3-api.us-geo.objectstorage.softlayer.net
Content-Length: 237
```

```xml
<CORSConfiguration>
  <CORSRule>
    <AllowedOrigin>http:www.ibm.com</AllowedOrigin>
    <AllowedMethod>GET</AllowedMethod>
    <AllowedMethod>PUT</AllowedMethod>
    <AllowedMethod>POST</AllowedMethod>
  </CORSRule>
</CORSConfiguration>
```

**Sample Response:**

```http
HTTP/1.1 200 OK
Date: Wed, 5 Oct 2016 15:39:38 GMT
X-Clv-Request-Id: 7afca6d8-e209-4519-8f2c-1af3f1540b42
Accept-Ranges: bytes
Server: Cleversafe/3.9.0.129
X-Clv-S3-Version: 2.5
x-amz-request-id: 7afca6d8-e209-4519-8f2c-1af3f1540b42
Content-Length: 0
```

#### Delete any cross-origin resource sharing configuration for a bucket

A `DELETE` issued to a bucket with the proper parameters creates or replaces a cross-origin resource sharing (CORS) configuration for a bucket.

**Syntax**

```bash
DELETE http://{endpoint}/{bucket-name}?cors=
```

**Sample Request:**

This is an example of deleting a CORS configuration for a bucket.

```http
GET /apiary?cors= HTTP/1.1
Authorization: {authorization-string}
x-amz-date: 20161011T190354Z
Host: s3-api.us-geo.objectstorage.softlayer.net
```

The server responds with `204 No Content`.

### Operations on Objects
{: #operations-on-objects}

#### Upload an object

A `PUT` given a path to an object uploads the request body as an object. A SHA256 hash of the object is a required header.


**Syntax**

```bash
PUT http://{endpoint}/{bucket-name}/{object-name}
```

**Sample Request**

```http
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
 the colony will fight fiercely to protect her. 

```

**Sample Response**

```http
HTTP/1.1 200 OK
Date: Thu, 25 Aug 2016 18:30:02 GMT
X-Clv-Request-Id: 9f0ca49a-ae13-4d2d-925b-117b157cf5c3
Accept-Ranges: bytes
Server: Cleversafe/3.9.0.121
X-Clv-S3-Version: 2.5
x-amz-request-id: 9f0ca49a-ae13-4d2d-925b-117b157cf5c3
ETag: "3ca744fa96cb95e92081708887f63de5"
Content-Length: 0
```

#### Get an objects headers

A `HEAD` given a path to an object retrieves that object's headers.

**Syntax**

```bash
HEAD http://{endpoint}/{bucket-name}/{object-name}
```

**Sample Request**

```http
HEAD /apiary/soldier-bee HTTP/1.1
Authorization: {authorization-string}
x-amz-date: 20160825T183244Z
Host: s3-api.sjc-us-geo.objectstorage.softlayer.net
Connection: close
```

**Sample Response**

```http
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
```

#### Download an object

A `GET` given a path to an object uploads an object.

**Syntax**

```bash
GET http://{endpoint}/{bucket-name}/{object-name}
```

**Sample Request**

```http
GET /apiary/worker-bee HTTP/1.1
Authorization: {authorization-string}
Host: s3-api.us-geo.objectstorage.softlayer.net
Connection: close
```

**Sample Response**

```http
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
```

#### Delete an object

A `DELETE` given a path to an object deletes an object.

**Syntax**

```bash
DELETE http://{endpoint}/{bucket-name}/{object-name}
```

**Sample Request**

```http
DELETE /apiary/soldier-bee HTTP/1.1
Authorization: {authorization-string}
Host: s3-api.sjc-us-geo.objectstorage.softlayer.net
Connection: close
```

**Sample Response**

```http
HTTP/1.1 204 No Content
Date: Thu, 25 Aug 2016 17:44:57 GMT
X-Clv-Request-Id: 8ff4dc32-a6f0-447f-86cf-427b564d5855
Accept-Ranges: bytes
Server: Cleversafe/3.9.0.121
X-Clv-S3-Version: 2.5
x-amz-request-id: 8ff4dc32-a6f0-447f-86cf-427b564d5855
```


