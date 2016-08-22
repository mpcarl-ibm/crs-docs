---
layout: page
title:  "Overview"
featured: true
weight: 1
tags: []
author: Nick
dateAdded: January 14th, 2016
---

The following table describes which bucket operations are supported in COS Dedicated IBM Managed API. It also indicates some operations that, while not supported, can be accomplished using another mechanism such as the Manager Web Interface or are planned for inclusion in future API versions. Any S3 API operation that is not present is not supported.

**Bucket operations:**

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