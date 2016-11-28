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

### About the `authorization` header
Each request made against IBM COS using the S3 API must be authenticated using an implementation of the AWS `authorization` header.  IBM COS supports Signature Version 2 and Signature Version 4 authentication methods.  Signature Version 4 is considered more secure as it does not include the secret access key itself as part of the signature but instead uses a . Using a signature provides identity verification and in-transit data integrity, and because each signature is tied to the timestamp of the request it is not possible to reuse authorization headers.  The header is composed of four components: an algorithm declaration, credential information, signed headers, and the calculated signature:  

```
AWS4-HMAC-SHA256 Credential={access-key}/{date}/{region}/s3/aws4_request,SignedHeaders=host;x-amz-date;{other-required-headers},Signature={signature}
```

The date is provided in `YYYYMMDD` format, and for COS Cross-Region the region should be `us-standard`. The `host` and `x-amz-date` headers are always required, and depending on the request other headers may be required as well (e.g. `x-amz-content-sha256` in the case of requests with payloads).  Due to the need to recalculate the signature for every individual request, many developers prefer to use a tool or SDK that will produce the authorization header automatically.

### Creating an `authorization` header

First we need to create a request in a standardized format.

1. Declare which HTTP method we are using (e.g. `PUT`)
2. Define the resource we are accessing in a standardized fashion.  This is the part of the address in between `http(s)://` and the query string.  For requests at the account level (i.e. listing buckets) this is simply `/`.
3. If there are any request parameters they must be standardized by being percent-encoded (e.g. spaces should be represented as `%20`) and alphabetized.
4. Headers need to be standardized by removing whitespace, converting to lowercase, and adding a newline to each, then they must be sorted in ASCII order. 
5. After being listed in a standard format, they must be 'signed'.  This is taking just the header names, not their values, and listing them in alphabetical order, separated by semicolons. `Host` and `x-amz-date` are required for all requests.
6. If the request has a body, such as when uploading an object or creating a new ACL, the request body must be hashed using the SHA-256 algorithm and represented as base-16 encoded lowercase characters.
7. Combine the HTTP method, standardized resource, standardized parameters, standardized headers, signed headers, and hashed request body each separated by a newline to form a standardized request.  

Next we need to assemble a 'string-to-sign' which will be combined with the signature key to form the final signature. The string-to-sign takes the following form:

```
AWS4-HMAC-SHA256
{time}
{date}/{region}/s3/aws4_request
{hashed-standardized-request}
```

1. The time must be current UTC and formatted according to the ISO 8601 specification (e.g. `20161128T152924Z`).
2. The date is in `YYYYMMDD` format.

TASK 3: CALCULATE THE SIGNATURE
Create the signing key using the function defined above.
Sign the string_to_sign using the signing_key

TASK 4: ADD SIGNING INFORMATION TO THE REQUEST
The signing information can be either in a query string value or in 
a header named Authorization. This code shows how to use a header.
Create authorization header and add to request headers
The request can include any headers, but MUST include "host", "x-amz-date", 
and (for this scenario) "Authorization". "host" and "x-amz-date" must
be included in the canonical_headers and signed_headers, as noted
earlier. Order here is not significant.
Python note: The 'host' header is added automatically by the Python 'requests' library.


### Example of generating an `authorization` header

```python
import os
import datetime
import hashlib
import hmac
import requests

# please don't store credentials directly in code
access_key = os.environ.get('COS_S3_ACCESS_KEY_ID')
secret_key = os.environ.get('COS_S3_SECRET_ACCESS_KEY')

# request elements
http_method = 'GET'
host = 's3-api.us-geo.objectstorage.softlayer.net'
region = 'us-standard'
endpoint = 'http://s3-api.us-geo.objectstorage.softlayer.net'
bucket = ''
object_key = ''
request_parameters = ''


# hashing and signing methods
def hash(key, msg):
    return hmac.new(key, msg.encode('utf-8'), hashlib.sha256).digest()


def createSignatureKey(key, datestamp, region, service):
    keyDate = hash(('AWS4' + key).encode('utf-8'), datestamp)
    keyRegion = hash(keyDate, region)
    keyService = hash(keyRegion, service)
    keySigning = hash(keyService, 'aws4_request')
    return keySigning


# assemble the standardized request
time = datetime.datetime.utcnow()
timestamp = time.strftime('%Y%m%dT%H%M%SZ')
datestamp = time.strftime('%Y%m%d')

standardized_resource = bucket + '/' + object_key
standardized_querystring = request_parameters
standardized_headers = 'host:' + host + '\n' + 'x-amz-date:' + timestamp + '\n'
signed_headers = 'host;x-amz-date'
payload_hash = hashlib.sha256('').hexdigest()

standardized_request = (http_method + '\n' +
                        standardized_resource + '\n' +
                        standardized_querystring + '\n' +
                        standardized_headers + '\n' +
                        signed_headers + '\n' +
                        payload_hash)


# assemble string-to-sign
hashing_algorithm = 'AWS4-HMAC-SHA256'
credential_scope = datestamp + '/' + region + '/' + 's3' + '/' + 'aws4_request'
sts = (hashing_algorithm + '\n' +
       timestamp + '\n' +
       credential_scope + '\n' +
       hashlib.sha256(standardized_request).hexdigest())


# generate the signature
signature_key = createSignatureKey(secret_key, datestamp, region, 's3')
signature = hmac.new(signature_key,
                     (sts).encode('utf-8'),
                     hashlib.sha256).hexdigest()


# assemble all elements into the 'authorization' header
v4auth_header = (hashing_algorithm + ' ' +
                 'Credential=' + access_key + '/' + credential_scope + ', ' +
                 'SignedHeaders=' + signed_headers + ', ' +
                 'Signature=' + signature)


# create and send the request
headers = {'x-amz-date': timestamp, 'Authorization': v4auth_header}
# the 'requests' package autmatically adds the required 'host' header
request_url = endpoint + standardized_resource + standardized_querystring

print '\nSending `%s` request to IBM COS -----------------------' % http_method
print 'Request URL = ' + request_url
request = requests.get(request_url, headers=headers)

print '\nResponse from IBM COS ----------------------------------'
print 'Response code: %d\n' % request.status_code
print request.text
```