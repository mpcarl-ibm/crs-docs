---
title: Billing information 
keywords: 
last_updated: November 18, 2016
tags: 
summary: 
sidebar: crs_sidebar
permalink: crs-billing.html
folder: crs
toc: false
---

{% include note.html content="Information on pricing can be found at [SoftLayer](https://www.softlayer.com/Store/orderService/objectStorage){: new_window}." %}

### Invoices
Invoices can be found at **Account** > **Billing** > **Invoices** in the navigation menu.

{% include tip.html content="Each account receives a single bill. If you need separate billing for different sets of containers, then creating multiple accounts is necessary." %}

### What is the difference between 'Class A' and 'Class B' requests?

'Class A' requests are operations that involve modification or listing.  This includes creating buckets, uploading or copying objects, creating or changing configurations, listing buckets, and listing the contents of buckets.

'Class B' requests are those related to retrieving objects or their associated metadata/configurations from the system.

There is no charge for deleting buckets or objects from the system.

| Class | Requests | Examples |
|--- |--- |--- |
| Class A | PUT, COPY, and POST requests, as well as GET requests used to list buckets and objects | Creating buckets, uploading or copying objects, listing buckets, listing contents of buckets, setting ACLs, and setting CORS configurations |
| Class B | GET (excluding listing), HEAD, and OPTIONS requests | Retrieving objects and metadata |
