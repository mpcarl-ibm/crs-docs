---
title: Swift
keywords:
last_updated: May 5, 2017
tags:
summary: Current customers of Softlayer Object Storage based on OpenStack Swift can see benefits moving to IBM COS
# TODO - What benefits?
sidebar: crs_sidebar
permalink: swift-migration
folder: swift
toc: True
---
## Benefits
### Cross region or availability zone
IBM Cloud ObjectStorage (COS) provides two ...

### S3 compatible API
IBM Cloud ObjectStorage (COS) provides an Amazon S3 [compatible API][1]

Openstack Swift provides a [Swift/APIFeatureComparison][2] document to help understand the high level similarities and differences between the APIs.

## Swift migration considerations
Migration from Swift to IBM Cloud ObjectStorage (COS)

## Amazon S3 migration considerations

### Use cases
#### Archive workloads

#### Static web hosting / CDN (future)


### API differences
Enabling public access to objects and containers/buckets

| Function | Swift    | COS      |
| -------- | -------- | -------- |
| Publicly readable object | Oranges | Pears |


### Data migration
Tools of interest

####Swift virtual file system (svfs)
The [Swift virtual file system][3] can be used to mount a swift container as a file system on Linux and MacOS systems. Once mounted, the objects in the container can be interacted with like standard files on the system.

####s3fs-fuse
Similarly, the [S3fs-fuse][4] utility can be use to mount a COS bucket as a files system on Linux or MacOS systems. The objects in the bucket can be

####Swift command line Tool




[1]: api-reference
[2]: https://wiki.openstack.org/wiki/Swift/APIFeatureComparison
[3]: https://github.com/ovh/svfs
