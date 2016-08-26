---
layout: page
title:  "API Overview"
featured: true
weight: 1
tags: []
author: Nick
dateAdded: August 18th, 2016
---

IBM Cloud Object Storage uses two seperate APIs:

1. Account administration using the SoftLayer API
2. Interacting with the object store using an S3 API

More information on using the SoftLayer API can be found at the [SoftLayer Development Network](http://sldn.softlayer.com/reference/services/SoftLayer_Network_Storage_Hub_Cleversafe_Account).

The following tables describe the complete set of supported operations when using the S3 interface to IBM Cloud Object Storage. 

### Operations on the service

The only operation that is applied directly to account level is to get a list of buckets owned by that account. Accounts are limited to 100 buckets.

| Service Operation | Note |
|:----|:---|
| GET Service | Used to retrieve of list of all buckets belonging to an account. | 

### Operations on buckets

These operations create, destroy, get information about, and control behavior of buckets.

| Bucket Operation | Note |
|:----|:---|
| DELETE Bucket | Deletes a bucket.  Requires ownership or ``FULL_CONTROL`` permissions. |
| DELETE Bucket CORS | Requires ownership or ``FULL_CONTROL`` permissions. |
| DELETE Bucket tagging | |
| GET Bucket | Lists objects contained in a bucket.  Limited to 1000 objects. |
| GET Bucket ACL |Requestor must be granted ``READ_ACL`` permission for bucket. |
| GET Bucket CORS |Requestor must be granted ``FULL_CONTROL`` permission for bucket. |
| GET Bucket location | Response will always contain a location constraint of empty string. |
| GET Bucket Tagging | |
| GET Bucket Request Payment |  |
| HEAD Bucket |  |
| List Multipart Uploads |  |
| PUT Bucket | Buckets must have unique names, and accounts are limited to 100 buckets. |
| PUT Bucket ACL | Requestor must be granted ``WRITE_ACL`` permission for bucket. |
| PUT Bucket CORS | Requestor must be granted ``FULL_CONTROL`` permission for bucket. |
| PUT Bucket Tagging | |
| PUT Bucket Request Payment | |

#### Operations on objects

These operations create, destroy, get information about, and control behavior of buckets.

| Object Operation | Note |
| :---------------| :------|
| DELETE Object |
| DELETE Multiple Objects  |
| GET Object |
| GET Object ACL |
| HEAD Object |
| OPTIONS Object |
| POST Object |
| PUT Object |
| PUT Object ACL |
| PUT Object (Copy) | Copying objects can result in high bandwidth utilization within the system until the copy is complete. |
| Initiate Multipart Upload |
| Upload Part |
| Upload Part (Copy) |
| Complete Multipart Upload |
| Abort Multipart Upload |
| List Parts |