---
title: Connecting to IBM COS Cross-Region
keywords: 
last_updated: November 18, 2016
tags: 
summary: 
sidebar: crs_sidebar
permalink: crs-connecting.html
folder: crs
---

Connecting to IBM COS requires specifying two fundamental pieces of information: credentials and an endpoint. Many tools that are compatible with the S3 API default to connecting to AWS endpoints, and so it is necessary to explicitly declare the endpoint when using these tools.

Credentials consist of two strings: an access key, and a secret key.  The access key is somewhat like a temporary account ID, and the secret key is essentially a password.  It is important not to accidentally compromise your application's security by inadvertently checking your credentials into a code repository! The easiest way to keep track of your keys is to set them as environment variables in your development environment.  This allows them to be called within your code but doesn't require them to be explicitly declared.