---
title: Managing objects
keywords: 
tags:
sidebar: bx_sidebar
permalink: bx-objects
summary: 
toc: false
---

From the object storage UI you can see the region in which you are working, as well as manage buckets, objects, and service credentials for your instance. You must be logged in to the org and space of the storage instance that you want to work with.

### Storing objects in buckets

**Attention**: When you upload a file with the same name, the service replaces the stored file with the new file. If you download a file and make edits, be sure to give the file another name before you upload to keep both versions of the file.

1. In your service dashboard, create a bucket and select **Add File**. A dialog window opens.
2. Use one of the following upload methods to add objects to your bucket.
    - To upload a single file, select the file and click **Open**.
    - To upload multiple files, select all of the files you would like to upload and then drag and drop them in the file table area of your bucket.


### Downloading objects from storage

1. In your service dashboard, select **Download File** to retrieve your object from storage.

### Deleting objects and buckets

**Note**: Only empty buckets can be deleted.

1. In your service instance dashboard, select **Delete File** to remove objects from your bucket.
2. If you no longer have use for your bucket, select **Delete Bucket**.


## Managing with the API

If you prefer, you can use the S3 API to manage objects and buckets. In order to access your {{site.data.keyword.cosdimshort}} instance, you must get the following information from your service credentials:

- Endpoint URL
- Access key ID
- Secret access key
- Region

For more information about getting started with the API, see the [overview documentation](https://ibm-public-cos.github.io/crs-docs/crs-api-reference.html).