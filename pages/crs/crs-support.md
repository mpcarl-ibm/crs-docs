---
title: Support and FAQ
keywords: 
last_updated: July 3, 2016
tags: 
summary: 
sidebar: crs_sidebar
permalink: crs-support.html
folder: crs
---

> IBM COS Standard Cross Region is currently in open trial.  Please visit [IBM Cloud](https://www.softlayer.com/Store/orderService/objectStorage) to participate.

Customers requiring support can open a ticket and check the status of an issue in the [customer portal](https://control.softlayer.com/){: new_window}.

## Frequently asked questions

#### What is the maximum number of buckets per storage account?  What if I need more?

Currently each storage account is limited to 100 buckets.  If more buckets are required, and creating another storage account is not a workable solution, please contact customer support.

#### How many objects can fit in a single bucket?

There is no practical limit to the number of objects in a single bucket.

#### Can I nest buckets inside one another?

No, buckets can not be nested.  If a greater level of organization is required within a bucket, the use of prefixes is supported: `{endpoint}/{bucket-name}/{object-prefix}/{object-name}`.  Note that the object's key remains the combination `{object-prefix}/{object-name}` and that
