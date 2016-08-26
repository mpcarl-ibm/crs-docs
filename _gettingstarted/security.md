---
layout: page
title:  "Data Security Overview"
featured: true
weight: 4
tags: []
author: Nick
dateAdded: August 18th, 2016
---

IBM Cloud Object Storage uses Information Dispersal Algorithms (IDAs) to separate data into unrecognizable “slices” that are distributed across a network of data centers, making transmission and storage of data inherently private and secure. No complete copy of the data resides in any single storage node, and only a subset of nodes needs to be available in order to fully retrieve the data on the network.

Data is secured by TLS in transit from clients to the data center as well as between devices within and between data centers.  Data at rest is secured through provider side encryption; this is default for all customer accounts.

When an object is deleted by a customer, enough slices of that object are immediately deleted to prevent the object from being read, and the remaining slices are overwritten by new data as it is written to disk.  Forensic reconstruction of an object from remaining encypted slices is virtually impossible.
