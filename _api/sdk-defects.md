---
layout: page
title:  "Tools and SDKs"
featured: false
weight: 3
tags: []
author: Nick
dateAdded: August 18th, 2016
---

# Java

The AWS SDK for Java can be cloned and built from source using Maven, or downloaded directy from AWS. You can learn about, download, and install Maven from [maven.apache.org](https://maven.apache.org/).

**To build from source**

1. From the command line, run `git clone https://github.com/aws/aws-sdk-java.git`.
2. Change to the newly cloned directory with `cd aws-sdk-java`.
3. Run `mvn clean install`.  To skip GPG-signing, run `mvn clean install -Dgpg.skip=true`.
4. Build detailed documentation by running `mvn javadoc:javadoc`.