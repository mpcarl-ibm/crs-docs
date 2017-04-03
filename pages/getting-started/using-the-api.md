---
title: Using the API
keywords: 
last_updated: November 18, 2016
tags:
summary: 
sidebar: crs_sidebar
permalink: using-the-api
redirect_from:
  - /crs-about-api
  - /crs-about-api.html
  - /crs-control-endpoints
  - /crs-control-endpoints.html
  - /crs-connecting
  - /crs-connecting.html
folder: getting-started
toc: True
---

Connecting to IBM COS via the API requires specifying two fundamental pieces of information: credentials and an endpoint. Many tools that are compatible with the S3 API default to connecting to AWS endpoints, and so it is necessary to explicitly declare the endpoint when using these tools.

Credentials consist of two strings: an access key, and a secret key.  The access key is somewhat like a temporary account ID, and the secret key is essentially a password.  It is important not to accidentally compromise your application's security by inadvertently checking your credentials into a code repository! The easiest way to keep track of your keys is to set them as environment variables in your development environment.  This allows them to be called within your code but doesn't require them to be explicitly declared.

At present two sets of credentials can be created per COS instance, allowing credentials to be rotated in applications without interruption.

For more detail on authentication and authorization, see the [Managing access]( {{ site.baseurl }}/manage-access) section.

{% include note.html content="The user interface portal provides a high level view of a storage account.  It is possible to view credentials and endpoints using the portal as well as by using the Softlayer API." %}

### Using the control portal to view credentials and endpoints
1. Credentials and available endpoints can be viewed, by clicking the  **View Credentials** link on the left side of an Account Details page (the view that shows a list of buckets in the account).
2. Click the credentials header to expand and show the credentials.
3. An account can have a maximum of two credentials at once. This allows for credentials to be rotated in applications without interruption. A new credential can be created, by clicking the **Add credential** button just below the Credentials header.
4. A credential can also be deleted, by clicking the **-** button to the right of the credential. A confirmation dialog will pop-up for confirmation before the credential is deleted.
5. The page also shows the various authentication endpoints that can be used to access the account. 

### Using libraries and SDKs

IBM Cloud does not provide native libraries or SDKs for interacting with COS offerings at this time, instead 2stored data is accessed using an implementation of the S3 API. Compatibility with an established object storage API allows developers to make use of a large ecosystem of third-party tools and SDKs.  

A full list of supported S3 API operations can be found in the [API Overview]( {{ site.baseurl }}/about-compatibility-api).

This documentation provides basic introductions to using popular third-party S3 API [client applications]( {{ site.baseurl }}/desktop-clients) and a [command line interface]( {{ site.baseurl }}/cli), as well as a library for the [Python]( {{ site.baseurl }}/crs-python.html) programming language. 
 

