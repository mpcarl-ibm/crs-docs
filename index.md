---
title: IBM Cloud Object Storage
keywords: 
tags:
sidebar: crs_sidebar
permalink: index
summary: 
toc: false
---


{% include tip.html content="IBM Cloud Object Storage (US Cross Region) is now offering a limited Free Tier for new and existing customers with [promotional code `COSFREE`](https://www.ibm.com/cloud-computing/bluemix/cloud-object-storage)." %}

### IBM COS Cross Region 
Information stored with IBM COS Cross Region is encrypted and dispersed across three geographic regions (Dallas, San Jose, and Washington, DC) and accessed through an implementation of the S3 API. This is the first public cloud storage service that makes use of the distributed storage technologies provided by IBM's Cloud Object Storage System. 

Cross Region service provides higher durability and availability than using a single region at the cost of slightly higher latency.  If a given data center is unavailable, the object store continues to function without impediment, and any missed changes are applied when the data center comes back online.

Developers use IBM COS's implementation of the S3 API to interact with their storage accounts. This documentation provides support to get started with provisioning accounts, to create buckets, to upload objects, and to use a reference of common API interactions. This offering is managed through the IBM Bluemix Infrastructure (formerly SoftLayer) Control portal.

### Other IBM object storage services

In addition to COS Cross Region, IBM Cloud currently provides three unique object storage offerings for different user needs, all of which are accessible through a web-based portal and RESTful APIs.

| Offering | Interface | Defining advantage |
|-- |-- |-- |
| IBM COS Cross Region | S3 API | -- Highest availability, durability and resiliency<br> -- Encryption at rest and in flight|
| Cloud Object Storage Regional | Swift API | -- Native integration with OpenStack clouds <br> -- Choice of 20 geographic regions|
| Object Storage for IBM Bluemix Developer Services | Swift API | -- Native integration with Bluemix services |
{:.overviewtable}

#### Cloud Object Storage Regional

Information stored with IBM COS Regional is located in one of 20 global data centers. Based on the OpenStack Swift platform, developers use the community Swift API to interact with their storage accounts. This offering is managed through the IBM Bluemix Infrastructure Control portal and does not provide encryption at-rest.

#### Object Storage for IBM Bluemix Developer Services

Information stored with Object Storage for IBM Bluemix is located in either Dallas or London data centers, and storage accounts are available for binding to Bluemix services. Based on the OpenStack Swift platform, developers use the community Swift API to interact with their storage accounts. This offering is managed through the IBM Bluemix console and does not provide encryption at-rest.

### About this documentation

This documentation is for anyone interested in learning about how to use IBM COS Cross Region to manage unstructured data I/O in their applications. For more detailed documentation about the other two object storage services, please visit the links found under **Documentation** in the header of this site.

Please contact `nicholas.lange@ibm.com` with questions or suggestions about the documentation itself.

