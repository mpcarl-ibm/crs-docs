---
title: Using storage classes  
keywords: 
last_updated: November 18, 2016
tags: 
summary: 
sidebar: crs_sidebar
permalink: /storage-classes
folder: how-to
---

Not all data that is stored needs to be accessed frequently, and some archival data might be rarely accessed if at all.  For less active workloads, buckets can be created in a different storage class and objects stored in these buckets will incur charges on a different schedule than standard storage.

There are three storage classes:

*  **Standard**: Used for active workloads - there is no charge for data retrieved (besides the cost of the operational request itself).
*  **Vault**: Used for cool workloads - an additional retrieval charge is applied each time data is read. The service includes a minimum threshold for object size and storage period consistent with the intended use of this service for cooler, less-active data. 
*  **Cold Vault**: Used for cold workloads - an larger additional retrieval charge is applied each time data is read. The service includes a longer minimum threshold for object size and storage period consistent with the intended use of this service for cold, inactive data. 
