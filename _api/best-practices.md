---
layout: page
title:  "Best practices"
featured: True
weight: 1
tags: []
author: Nick
dateAdded: August 18th, 2016
---

### Object naming
It is recommended to randomize the beginning component of the object name to avoid performance.  Randomness ensures that the index does not become unbalanced and can lead to significant performance gains.  

Example of **inefficient** naming scheme:

```bash
PUT /bucket1/filename-00001
PUT /bucket1/filename-00002
PUT /bucket1/filename-00003
PUT /bucket1/filename-00004
PUT /bucket1/filename-00005
```

Examples of an **efficient** naming scheme:

```bash
PUT /bucket1/c1ca2-filename-00001
PUT /bucket1/c9872-filename-00002
PUT /bucket1/98837-filename-00003
PUT /bucket1/abfc4-filename-00004
PUT /bucket1/9ac18-filename-00005
```

### Tuning cipher settings
IBM COS supports variety of cipher settings to encrypt data in transit. Not all cipher settings yield the same level performance. Negotiating one of `TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384`, `TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA`, `TLS_RSA_WITH_AES_256_CBC_SHA`, `TLS_RSA_WITH_AES_128_CBC_SHA` has shown to yield the same levels of performance as no TLS between the client and the IBM COS.

### Using multipart uploads
If the objects being stored in IBM COS are all larger objects, it is recommend that multipart upload operation is used to write objects into IBM COS.  Multipart uploads are only available for objects larger than 5MB - depending on the object size a part size of 20MB to 100MB is recommended. 

>**NOTE**: Using more than 500 parts leads to inefficiencies in IBM COS and should be avoided.

The multipart upload s designed to improve the upload performance for larger objects.  An upload of a single object can be performed as a set of parts and these parts can be uploaded independently in any order, and in parallel.  Upon upload completion, IBM COS then presents all parts as a single object.

Using multipart upload provides the following advantages:

* **Improved throughput**—You can upload parts in parallel to improve throughput. 
* **Quick recovery from any network issues**—Smaller part size minimizes the impact of restarting a failed upload due to a network error. 
* **Pause and resume object uploads**—Upload object parts over time. Once a multipart upload is initiated a multipart upload there is no expiry; it must explicitly complete or the multipart upload has to be aborted.
* **Begin an upload before the final object size is known**—An object can be uploaded as it is being created.

Due to the additional complexity of multipart uploads, as a best practice; it is recommended that S3 API libraries that offer multipart uploads be used. 

>**NOTE**: Incomplete multipart uploads do persist until the object is deleted or the multipart upload is aborted with `AbortIncompleteMultipartUpload`. If an incomplete multipart upload is not aborted, the partial upload continues to use resources.  Interfaces should be designed with this point in mind, and clean up incomplete multipart uploads.  

### Using software development kits

It is not mandatory to use published S3 API SDKs; homegrown libraries can be written to integrate with IBM COS. However, using published S3 API libraries provide advantages such as automatic retry logic on `5xx` errors. If using home grown libraries to integrate with IBM COS, care must be taken to handle transient errors such as `503`s originating from IBM COS. Retries with exponential backing similar to published SDKs must be implemented in these libraries.

