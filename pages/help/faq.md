---
title: Frequently asked questions
keywords: 
last_updated: November 18, 2016
tags: 
summary: 
sidebar: crs_sidebar
permalink: faq
folder: help
toc: false
---

#### What is the maximum number of buckets per storage account?  What if I need more?

Currently each storage account is limited to 100 buckets.  If more buckets are required, and creating another storage account is not a workable solution, please contact customer support.

#### How many objects can fit in a single bucket?

There is no practical limit to the number of objects in a single bucket.

#### Can I nest buckets inside one another?

No, buckets can not be nested.  If a greater level of organization is required within a bucket, the use of prefixes is supported: `{endpoint}/{bucket-name}/{object-prefix}/{object-name}`.  Note that the object's key remains the combination `{object-prefix}/{object-name}`.

#### What is the difference between 'Class A' and 'Class B' requests?

'Class A' requests are operations that involve modification or listing.  This includes creating buckets, uploading or copying objects, creating or changing configurations, listing buckets, and listing the contents of buckets.

'Class B' requests are those related to retrieving objects or their associated metadata/configurations from the system.

There is no charge for deleting buckets or objects from the system.
