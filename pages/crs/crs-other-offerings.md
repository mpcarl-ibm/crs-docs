---
title: Other IBM object storage options
keywords: 
last_updated: November 18, 2016
tags:
summary: 
sidebar: crs_sidebar
permalink: crs-other-offerings.html
folder: crs
toc: false
---

IBM Bluemix Infrastructure (formerly SoftLayer) currently offers three unique object storage offerings for different user needs, all of which are accessible through a web-based portal and RESTful APIs.

| Offering | Interface | Defining advantage |
|-- |-- |-- |
| IBM COS Standard Cross-Region | S3 API | -- Highest availability, durability and resiliency<br> -- Encryption at rest and in flight|
| IBM COS Standard Regional | Swift API | -- Native integration with OpenStack clouds <br> -- Choice of 20 geographic regions|
| Object Storage for IBM Bluemix | Swift API | -- Native integration with Bluemix services |
{:.overviewtable}

### IBM COS Standard Regional

Information stored with IBM COS Standard Regional is located in one of 20 global data centers. Based on the OpenStack Swift platform, developers use the community Swift API to interact with their storage accounts. This offering is managed through the IBM Bluemix Infrastructure Control portal and does not provide encryption at-rest.

### Object Storage for IBM Bluemix

Information stored with Object Storage for IBM Bluemix is located in either Dallas or London data centers, and storage accounts are available for binding to Bluemix services. Based on the OpenStack Swift platform, developers use the community Swift API to interact with their storage accounts. This offering is managed through the IBM Bluemix console and does not provide encryption at-rest.
 

