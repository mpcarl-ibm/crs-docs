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
* [Prerequisites](#prerequisite)
* [Common Headers and Error Responses](#headers-and-error-response)
* [Access Control Lists](#access-control-list)
* [Operations on the Service](#operations-on-service)
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

### Access Control Lists
{: #access-control-list}

COS Dedicated IBM Managed supports ACLs for both buckets and objects. ACL grantees can be specified using any of the following methods:
**Methods of Specifying Grantees for ACLs in COS Dedicated IBM Managed API:**
| Method | Description | Example |
|--------|-------------|---------|
| Canonical ID | User account UUID (found on Manager node) | 43a89ab8-a5e9-44bf-9671-d23a8729b2e0  |
| Email Address | Account username of user account (NOTE: Email address is an optional account attribute, so the account username is used in its place) | user1 |
| URI | Used for Amazon’s pre-defined groups. COS Dedicated IBM Managed supports the All Users Group for bucket ACLs and the All Users Group and Authenticated Users URIs for Object ACLs. All other predefined groups are unsupported. | http://acs.amazonaws.com/groups/global/AllUsers |
**Behavior of Permissions for ACLs in COS Dedicated IBM Managed API:**
| Permission | When granted on a bucket | When granted on an object |
|------------|--------------------------|---------------------------|
| READ | Allows grantee to list and read all objects in bucket | Allows grantee to read object data and metadata |
| WRITE | Allows grantee to create, overwrite and delete any object in bucket. Cannot be granted independently from READ permission. | N/A |
| READ_ACP | This permission does not exist for buckets; default setting is FULL_CONTROL | Allows grantee to read object ACL |
| WRITE_ACP | Default setting is FULL_CONTROL| Allows grantee to write ACL for applicable object |
| FULL_CONTROL | Allows grantee READ, WRITE, READ_ACP and WRITE_ACP permissions on bucket | Allows grantee READ, READ_ACP and WRITE_ACP permissions on object |

**Note:** The ``READ_ACP``, ``WRITE_ACP``, and ``FULL_CONTROL`` permissions are implied by the bucket “own” permission. When any of these permissions are assigned to a grantee in a bucket ACL, that grantee will be granted the bucket “own” permission.
**Canned ACL Support:**
| Canned ACL | Applies to | Supported |
|------------|------------|-----------|
| private | Bucket and object | Yes. When set on a bucket, the requestor is interpreted as the bucket owner. |
| public-read | Bucket and object | Yes. When set on a bucket, the requestor is interpreted as the bucket owner. |
| public-read-write | Bucket and object |Yes. When set on a bucket, the requestor is interpreted as the bucket owner. |
| authenticated-read  | Bucket and object | Supported when set on an object only. Not supported as a bucket ACL. |
| bucket-owner-read | Object  | No |
| bucket-owner-full-control | Object | No |
| log-delivery-write | Bucket | No. Bucket access logging feature not supported |
### Operations on the Service
{: #operations-on-service}

The following table describes the service operation supported by the COS Dedicated IBM Managed API. 
**Service operation supported by COS Dedicated IBM Managed:**

| Operation | Supported |
|-----------|----------------|
| GET Service (List Buckets) | Yes | 
#### GET Service

Unless otherwise noted in the following table, all parameters, operation-specific request and response headers and the response body conform to the S3 API.
This ``GET`` operation lists all buckets the authenticated requestor owns. Authenticating this request requires a valid S3 registered AWS Access Key ID. Lists cannot be generated by anonymous requests nor can they include buckets owned by other users.

**GET services supported by COS Dedicated IBM Managed:**

| Operation | Supported | Notes |
|-----------|-----------|-------|
| Request Headers | Yes | |
| Response Headers | Yes | |
| Response Elements | Yes |Bucket Owner and Bucket Creation Date not supported. A date of Thu, 01 Jan 1970 00:00:00 GMT will be returned. |
For more information, see [Common Headers](#headers-and-error-response).
### Operations on Buckets

The following table describes which bucket operations are supported in COS Dedicated IBM Managed API. It also indicates some operations that, while not supported, can be accomplished using another mechanism such as the Manager Web Interface or are planned for inclusion in future API versions. Any S3 API operation that is not present is not supported.
**Bucket operations supported by COS Dedicated IBM Managed:**
| Operation | Supported | Note |
|-----------|-----------|------|
| DELETE Bucket | Yes | Requestor must have Bucket Provisioner role assigned and be granted ``FULL_CONTROL`` permission for bucket. Provisioning API configuration must be enabled and set to ``CREATE`` and ``DELETE`` |
| DELETE Bucket CORS | Yes | Requestor must be granted ``FULL_CONTROL`` permission for bucket. |
| DELETE Bucket lifecycle | No | |
| DELETE Bucket policy | No | Use Manager API. |
| DELETE Bucket tagging | Yes | |
| DELETE Bucket website | No | |
| GET Bucket (List Objects) | Yes | |
| GET Bucket ACL | Yes |Requestor must be granted ``READ_ACL`` permission for bucket. |
| GET Bucket CORS | Yes |Requestor must be granted ``FULL_CONTROL`` permission for bucket. |
| GET Bucket lifecycle | No | |
| GET Bucket policy | No | Use Manager API. |
| GET Bucket location | Yes | Response will always contain a location constraint of empty string, consistent with Amazon’s representation of the US Classic Region. |
| GET Bucket Logging | No | Contact COS Dedicated IBM Managed Support to get audit logs.|
| GET Bucket Notification | No |  |
| GET Bucket Tagging | Yes | |
| GET Bucket Object Versions | Yes |  |
| GET Bucket Request Payment | Yes |  |
| GET Bucket Versioning |Yes | Requestor must be granted ``FULL_CONTROL`` permission for bucket. |
| GET Bucket Website | No |  |
| HEAD Bucket | Yes |  |
| List Multipart Uploads | Yes |  |
| PUT Bucket | Yes | Requester must have the Bucket Provisioner role assigned. Before using this operation, a Bucket Template must exist. If the S3 ``LocationConstraint`` parameter (as documented by Amazon) is used, it must correspond to the provisioning code for the template. If omitted, the default template will be used. |
| PUT Bucket ACL | Yes | Requestor must be granted ``WRITE_ACL`` permission for bucket. |
| PUT Bucket CORS | Yes | Requestor must be granted ``FULL_CONTROL`` permission for bucket. |
| PUT Bucket Lifecycle | No |  |
| PUT Bucket Policy | No |  Use Manager API. |
| PUT Bucket Logging | No | Contact COS Dedicated IBM Managed Support to get audit logs. |
| PUT Bucket Notification | No | |
| PUT Bucket Tagging | Yes | |
| PUT Bucket Request Payment | Yes | |
| PUT Bucket Versioning | Yes | Requestor must be granted ``FULL_CONTROL`` permission for bucket. |
| PUT Bucket Website | No | |



For more information, see [S3 API Operations on Buckets](http://docs.aws.amazon.com/AmazonS3/latest/API/RESTBucketOps.html).
#### GET Bucket

All parameters, operation-specific request and response headers and the response body conform to the S3 API under normal operations when the name index is enabled.

When Recovery Listing is enabled, the results of a ``GET`` Bucket (List Objects) operation are returned in non-lexicographical order. Pagination is supported using the normal convention by utilizing the marker parameter and ``nextMarker`` response element. A search using the prefix and delimiter parameters in recovery listing mode will result in an HTTP ``405 Method Not Allowed`` response code.
If the name index is disabled for a bucket and recovery mode listing is not enabled, all listing operation requests also will result in an HTTP ``405 Method Not Allowed`` response code.

COS Dedicated IBM Managed ignores any value greater than 1,000 for ``max-keys`` and returns only up to 1,000 keys.

For more information, see [Common Headers](#headers-and-error-response).
#### HEAD Bucket
There are no operation specific request headers or response headers for this operation.
For more information, see [Common Headers](#headers-and-error-response).

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
 
For more information, see [S3 API Operations on Objects.](http://docs.aws.amazon.com/AmazonS3/latest/API/RESTObjectOps.html) and [Common Headers](#headers-and-error-response).

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
