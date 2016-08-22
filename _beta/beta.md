---
layout: page
title: IBM Cloud Object Storage Documentation (Beta)
permalink: /beta/
category: top-level
top-level: gettingstarted
---

IBM Cloud Object Storage provides a high performance solution to the problem of managing increasingly vast amounts of unstructured data. Data is stored in the public cloud and accessed using either S3 or Swift interfaces.

Object storage does not use a traditional directory tree. Discrete units of data (objects) exist at the same level in a storage pool. Each object has a unique, identifying name that an application uses to retrieve it. Additionally, each object may have metadata that is retrieved with it.  Objects and metadata are accessed directly by applications using an Representational State Transfer (REST) web services API, rather than being directly manipulated by users.  

Using Cleversafe technology, information is encrypted, sliced, and dispersed across a system of storage devices.  This decentralized approach provides very high reliability, durability, and scalability, and allows for individual devices to be added, removed, or upgraded on the fly. Cleversafe technology is only available when using the S3 API, although Swift support is expected in early 2017.

For a detailed reference to the SoftLayer API, please see the [SoftLayer Development Network](http://sldn.softlayer.com/article/Softlayer-API-Overview).  Note that any references to object storage only pertains to a Swift implementation.




