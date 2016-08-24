---
layout: page
title:  "Overview"
featured: true
weight: 1
tags: []
author: Nick
dateAdded: August 18th, 2016
---

IBM Cloud Object Storage uses two seperate APIs: one for account administration using the SoftLayer API, and a second for interacting with the object store using an S3 API.

More information on using the SoftLayer API can be found at the [SoftLayer Development Network](http://sldn.softlayer.com/reference/services/SoftLayer_Network_Storage_Hub_Cleversafe_Account).

The following tables describe the complete set of supported operations when using the S3 interface to IBM Cloud Object Storage. 

#### Operations on the service

| Service Operation | Note |
|:----|:---|
| GET Service    (List buckets)| | 

#### Operations on buckets

| Bucket Operation | Note |
|:----|:---|
| DELETE Bucket | Requestor must have Bucket Provisioner role assigned and be granted ``FULL_CONTROL`` permission for bucket. Provisioning API configuration must be enabled and set to ``CREATE`` and ``DELETE`` |
| DELETE Bucket CORS | Requestor must be granted ``FULL_CONTROL`` permission for bucket. |
| DELETE Bucket tagging | |
| GET Bucket (List Objects) | |
| GET Bucket ACL |Requestor must be granted ``READ_ACL`` permission for bucket. |
| GET Bucket CORS |Requestor must be granted ``FULL_CONTROL`` permission for bucket. |
| GET Bucket location | Response will always contain a location constraint of empty string. |
| GET Bucket Tagging | |
| GET Bucket Object Versions |  |
| GET Bucket Request Payment |  |
| GET Bucket Versioning | Requestor must be granted ``FULL_CONTROL`` permission for bucket. |
| HEAD Bucket |  |
| List Multipart Uploads |  |
| PUT Bucket | Requester must have the Bucket Provisioner role assigned. Before using this operation, a Bucket Template must exist. If the  ``LocationConstraint`` parameter is used, it must correspond to the provisioning code for the template. If omitted, the default template will be used. |
| PUT Bucket ACL | Requestor must be granted ``WRITE_ACL`` permission for bucket. |
| PUT Bucket CORS | Requestor must be granted ``FULL_CONTROL`` permission for bucket. |
| PUT Bucket Tagging | |
| PUT Bucket Request Payment | |

#### Operations on objects

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