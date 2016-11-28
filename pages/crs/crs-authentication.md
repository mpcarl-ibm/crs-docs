---
title: Authorization and authentication
keywords: 
last_updated: November 18, 2016
tags: 
summary: 
sidebar: crs_sidebar
permalink: crs-authentication.html
folder: crs
---

### The `authorization` header
Each request made against IBM COS using the S3 API must be authenticated using an implementation of the AWS `authorization` header.  IBM COS supports Signature Version 2 and Signature Version 4 authentication methods.  Signature Version 4 is considered more secure as it does not include the secret access key itself as part of the signature. Using a signature provides identity verification and in-transit data integrity, and because each signature is tied to the timestamp of the request it is not possible to reuse authorization headers.  The header is composed of four components: an algorithm declaration, credential information, signed headers, and the calculated signature:  

```
AWS4-HMAC-SHA256 Credential={access-key}/{date}/{region}/s3/aws4_request,SignedHeaders=host;x-amz-date;{other-required-headers},Signature={signature}
```

The date is provided in `YYYYMMDD` format, and for COS Cross-Region the region should be `us-standard`. The `host` and `x-amz-date` headers are always required, and depending on the request other headers may be required as well (e.g. `x-amz-content-sha256` in the case of requests with payloads).  Due to the need to recalulate the signature for every individual request, many developers prefer to use a tool or SDK that will produce the authorization header automatically.


