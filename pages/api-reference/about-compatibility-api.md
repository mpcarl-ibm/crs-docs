---
title: Supported API operations
keywords: 
last_updated: November 18, 2016
tags: 
summary: 
sidebar: crs_sidebar
permalink: about-compatibility-api
folder: api-reference
---

IBM Cloud Object Storage provides two separate APIs for managing and using object storage:

* Account and credential administration uses the SoftLayer API
* Interacting with buckets and objects uses an implementation of the S3 API

More information on using the SoftLayer API to create or delete credentials, check capacity usage, retrieve a UUID, and other account functions can be found at the [SoftLayer Development Network](http://sldn.softlayer.com/reference/services/SoftLayer_Network_Storage_Hub_Cleversafe_Account).

The following tables describe the complete set of supported operations when using the S3 API to access IBM Cloud Object Storage.  For details on using the operations, including examples, see [the API reference page]({{ site.baseurl }}/crs-api-reference).

### Operations on the account

The only operation that is performed directly at the account level is to get a list of buckets owned by that account. Accounts are limited to 100 buckets.

| Account operation | Note |
|:----|:---|
| `GET` account | Used to retrieve a list of all buckets belonging to an account. |
{:.opstable}

### Operations on buckets

These operations create, destroy, get information about, and control behavior of buckets.

| Bucket operation | Note |
|:----|:---|
| `DELETE` Bucket | Deletes an empty bucket.   |
| `DELETE` Bucket CORS | Deletes any cross-origin resource sharing configuration set on a bucket. |
| `GET` Bucket | Lists objects contained in a bucket.  Limited to listing 1,000 objects at once. |
| `GET` Bucket ACL |Retrieves the access control list for a bucket.|
| `GET` Bucket CORS |Retrieves any cross-origin resource sharing configuration set on a bucket.|
| `HEAD` Bucket | Retrieves a bucket's headers. |
| `GET` multipart uploads | Lists multipart uploads that have not completed or been canceled. |
| `PUT` Bucket | Buckets have naming restrictions. Accounts are limited to 100 buckets. |
| `PUT` Bucket ACL | Creates an access control list for a bucket. |
| `PUT` Bucket CORS | Creates a cross-origin resource sharing configuration for a bucket.|
{:.opstable}

### Operations on objects

These operations create, destroy, get information about, and control behavior of objects.

| Object operation | Note |
| :---------------| :------|
| `DELETE` Object | Deletes an object from a bucket.
| `DELETE` Multiple Objects  | Deletes multiple objects from a bucket.
| `GET` Object | Retrieves an object from a bucket.
| `GET` Object ACL | Retrieves an object's access control list.
| `HEAD` Object | Retrieves an object's headers.
| `OPTIONS` Object | Checks CORS configuration to see if a specific request can be sent.
| `POST` Object | Adds an object to a bucket using HTML forms.
| `PUT` Object | Adds an object to a bucket.
| `PUT` Object ACL | Creates an access control list for an object. 
| `PUT` Object (Copy) | Creates a copy of an object. |
| Initiate Multipart Upload | Creates an upload ID for a given set of parts to be uploaded.
| Upload Part | Uploads a part of an object associated with an upload ID.
| Upload Part (Copy) | Uploads a part of an existing object associated with an upload ID.
| Complete Multipart Upload | Assembles an object from parts associated with an upload ID.
| Abort Multipart Upload | Aborts upload and deletes outstanding parts associated with an upload ID.
| List Parts | Returns a list of parts associated with an upload ID
{:.opstable}