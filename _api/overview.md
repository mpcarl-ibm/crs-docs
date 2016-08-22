---
layout: page
title:  "Overview"
featured: true
weight: 1
tags: []
author: Nick
dateAdded: August 18th, 2016
---



The following table describes which bucket operations are supported using the S3 API. It also indicates some operations that, while not supported, can be accomplished using another mechanism such as the Manager Web Interface or are planned for inclusion in future API versions. Any S3 API operation that is not present is not supported.

**Bucket operations:**

| Operation | Note |
|:----|:---|
| DELETE Bucket | Requestor must have Bucket Provisioner role assigned and be granted ``FULL_CONTROL`` permission for bucket. Provisioning API configuration must be enabled and set to ``CREATE`` and ``DELETE`` |
| DELETE Bucket CORS | Requestor must be granted ``FULL_CONTROL`` permission for bucket. |
| DELETE Bucket tagging | |
| GET Bucket (List Objects) | |
| GET Bucket ACL |Requestor must be granted ``READ_ACL`` permission for bucket. |
| GET Bucket CORS |Requestor must be granted ``FULL_CONTROL`` permission for bucket. |
| GET Bucket location | Response will always contain a location constraint of empty string, consistent with Amazon’s representation of the US Classic Region. |
| GET Bucket Tagging | |
| GET Bucket Object Versions |  |
| GET Bucket Request Payment |  |
| GET Bucket Versioning | Requestor must be granted ``FULL_CONTROL`` permission for bucket. |
| HEAD Bucket |  |
| List Multipart Uploads |  |
| PUT Bucket | Requester must have the Bucket Provisioner role assigned. Before using this operation, a Bucket Template must exist. If the S3 ``LocationConstraint`` parameter is used, it must correspond to the provisioning code for the template. If omitted, the default template will be used. |
| PUT Bucket ACL | Requestor must be granted ``WRITE_ACL`` permission for bucket. |
| PUT Bucket CORS | Requestor must be granted ``FULL_CONTROL`` permission for bucket. |
| PUT Bucket Tagging | |
| PUT Bucket Request Payment | |

**Object operations**

| Operation | Note |
| DELETE Object |
| DELETE Multiple Objects  |
| GET Object |
| GET Object ACL |
| HEAD Object |
| OPTIONS Object |
| POST Object |
| PUT Object |
| PUT Object ACL |
| PUT Object (Copy) | Copying objects results in a full-Width read and then a full-Width write. These full copies will result in high bandwidth utilization within the system until the copy is complete. |
| Initiate Multipart Upload |
| Upload Part |
| Upload Part (Copy) |
| Complete Multipart Upload |
| Abort Multipart Upload |
| List Parts |