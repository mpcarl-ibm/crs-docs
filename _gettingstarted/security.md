---
layout: page
title:  "Security overview"
featured: true
weight: 4
tags: []
author: Nick
dateAdded: August 18th, 2016
---

Cleversafe employs a dispersed storage algorithm that slices objects, encrypts the slices, and distributes them across a pool of storage devices.  

Various mechanisms exists which prevent recovery of a deleted object once a commit and finalization of the delete is successfully acknowledged back to the client. The deletion of an object undergoes various stages, from marking the meta-data indicating the object as deleted, to removing the content regions, to the finalization of the erasure on the drives themselves until the eventual overwriting the blocks representing that slice data. Depending on whether one compromised the data center or has possession of the physical disks, the time an object becomes unrecoverable depends on the phase of the delete operation. When the metadata object is updated, clients external from the data center network can no longer read the object. When a majority of slices representing the content regions have been finalized by the storage devices, it is not possible to access the object. Once the underlying blocks on a majority of the disks have been overwritten forensic extraction of the object becomes impossible.