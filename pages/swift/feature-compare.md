---
title: Feature comparison
keywords:
last_updated: May 5, 2017
tags:
summary: There are a number of Swift features that have equivalent functions in COS
sidebar: crs_sidebar
permalink: feature-compare
folder: swift
toc: True
---

## Available now
### Temporary, public access to objects
Swift and COS each allow temporary, public access to objects without having to change the underlying ACLs on the object. Swift refers to this as a "TempURL" while COS uses the S3 term "pre-signed urls." The following examples show how to give temporary public access to an object in Swift and an object in COS.

Swift CLI example, which displays the URL that can be used to access the object:
```shell
$ swift tempurl GET 600 /v1/CONTAINER/OBJECT_1 'KEY_ONE'
/v1/CONTAINER/OBJECT_1?temp_url_sig=735c8ee6c7c9fef88437f29d11ce3cf769a2565d&temp_url_expires=1495482380
```

COS example:
```shell
$ aws --endpoint-url=https://{endpoint} s3 presign s3://bucket-1/new-file --expires-in 600
```

See [Pre-signed URLs][2] for additional details.


### Container/Bucket Access Control Lists (ACLs)
Swift and COS provide the ability to control access to containers and buckets.

Basic Swift headers that can be set on containers:
* `X-Container-Read` makes the container publicly readable
* `X-Container-Write` makes the container publicly writeable
Both headers can be set for read & write access.

The Swift [ACLs][5] documentation provides complete documentation on using ACLs with Swift.

COS customers can use the pre-made permissions via the `x-amz-acl` headers:
* `private`
* `public-read`
* `public-read-write`

Additional details on COS permissions can be found in the [API reference][6] documentation.

Public read access for both Swift and COS can be used to host public web content directly from ObjectStorage. However, in many cases using the [CDN][7] may be more performant.


### Versioning
Swift and COS support versioning of objects, though there are a few differences

####  COS versioning

- All versions kept in same bucket
- Delete leaves a "delete marker" -- simple GET does not restore
- A version can be deleted with the ID
- All objects have a version ID as part of their MD -- null by default

####  Swift versioning

- Versions copied to a separate versions location container when overwritten on PUT
- Older versions renames to have a prefix of the length of the object name and suffix of /<creation time>
- Delete restores prior version by copying back and permanently deletes current version -- big semantic difference
- Need to delete all versions -- no easy way to delete all versions


####  Common

- Set at bucket level
- Older versions can be returned by specific request for a version (but URI will differ)


## Future functions

### Content Distribution Network (CDN)
Swift containers can be exposed via the Softlayer CDN network using the following command:
```shell
$ curl -X PUT 'https://endpoint/v1/ACCOUNT/CONTAINER' -H "X-Auth-Token: <token>" -H "X-Container-Read: .r:*" -H "*X-Context: cdn"
```
See [Swift CDN support][3] for more details.

***CDN is not currently available in COS.***

## Pierre still working on...
### Search

### API
### SDKs/Language bindings
### Versioning

### Expiry
### Large Objects / multipart upload
### Metadata
### Bulk upload
### Request headers per operation
### Response headers
### Bulk delete


[1]: https://developer.openstack.org/api-ref/object-storage/index.html?expanded=create-update-or-delete-container-metadata-detail#containers
[2]: cli#pre-signed-urls
[3]: http://sldn.softlayer.com/reference/Object-Storage-CDN/Create-CDN-enabled-Container
[4]: https://control.softlayer.com
[5]: https://docs.openstack.org/developer/swift/overview_acl.html#container-acls
[6]: api-reference
[7]: #content-distribution-network-cdn
