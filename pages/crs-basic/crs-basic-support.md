---
title: Support and FAQ
keywords: 
last_updated: November 18, 2016
tags: 
summary: 
sidebar: crs_basic_sidebar
permalink: crs-basic-support.html
folder: crs-basic
---

> IBM COS Standard Cross Region is currently in open trial. 
> 
> Please visit [IBM Cloud](https://www.softlayer.com/Store/orderService/objectStorage) to participate.

### Getting support

Customers requiring support can open a ticket and check the status of an issue in the [customer portal](https://control.softlayer.com/){: new_window}. Technical questions can be asked on [StackOverflow](http://stackoverflow.com/questions/tagged/object-storage+ibm-bluemix), and more general questions about IBM Cloud Object Storage can be asked on [dW Answers](https://developer.ibm.com/answers/smartspace/public-cloud-object-storage/).

### Frequently asked questions

#### What is the maximum number of buckets per storage account?  What if I need more?

Currently each storage account is limited to 100 buckets.  If more buckets are required, and creating another storage account is not a workable solution, please contact customer support.

#### How many objects can fit in a single bucket?

There is no practical limit to the number of objects in a single bucket.

#### Can I nest buckets inside one another?

No, buckets can not be nested.  If a greater level of organization is required within a bucket, the use of prefixes is supported: `{endpoint}/{bucket-name}/{object-prefix}/{object-name}`.  Note that the object's key remains the combination `{object-prefix}/{object-name}` and that
