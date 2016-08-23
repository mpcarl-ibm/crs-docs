---
layout: page
title:  "Credentials & Endpoints"
featured: true
weight: 2
tags: []
author: Nick
dateAdded: August 18th, 2016
---

#### Credentials 
1. Credentials and available endpoints can be viewed by clicking on the  ‘View Credentials’ link on the top right of the details page.
    ![Credentials]({{ site.baseurl }}/img/cr1.png)
2. Clicking on the credentials header would expand and show the credentials.
    ![Credentials]({{ site.baseurl }}/img/cr2.png)
3. An account can have a maximum of two credentials at once. A new credential can be created by clicking on the ‘Add credential’ button just below the Credentials header to the left
4. A credential can also be deleted by clicking on the ‘-’ button to the right of the credential. A confirmation dialog will pop-up for confirmation before the credential is deleted.
5. The page also shows the various authentication endpoints that can be used to access the account.

#### Endpoints

SoftLayer provides three regional endpoints for connecting S3 applications or clients to the public cloud. Endpoints should be chosen based on proximity to the application servers accessing the object store to minimize latency.  For workloads not concentrated in a single geographic area, the US Region endpoint distributes connections between the three regional data centers.  Endpoints can be accessed over either plaintext or SSL depending on security requirements.

```** INTERNAL DRAFT NOTE: will add code examples showing path-based and host-based addressing **```

There are three types of endpoint:

* **Public endpoints** can be accessed from anywhere, and customers are charged for outgoing bandwitdth. Incoming bandwidth is free.
* **Private endpoints** can be accessed by customers running Virtual Machines or Bare Metal Servers on SoftLayer. Private endpoints do not incur charges for any outgoing or incoming bandwidth even if the traffic is cross regions or across data centers. 
* **ADN endpoints** allow customers to bring their own IP addresses. If customer have an ADN account and use an ADN endpoint, they will not be charged for any outgoing or incoming bandwidth.

```** INTERNAL DRAFT NOTE: endpoints list will be formatted in a table **```

**Dallas**
public: `s3-api.dal-us-geo.objectstorage.softlayer.net`
private: `s3-api.dal-us-geo.objectstorage.service.networklayer.com`
adn: `s3-api.dal-us-geo.objectstorage.adn.networklayer.com`

**San Jose**
public: `s3-api.sjc-us-geo.objectstorage.softlayer.net`
private: `s3-api.sjc-us-geo.objectstorage.service.networklayer.com`
adn: `s3-api.sjc-us-geo.objectstorage.adn.networklayer.com`

**Washington**
public: `s3-api.wdc-us-geo.objectstorage.softlayer.net`
private: `s3-api.wdc-us-geo.objectstorage.service.networklayer.com`
adn: `s3-api.wdc-us-geo.objectstorage.adn.networklayer.com`

**US Region**
public: `s3-api.us-geo.objectstorage.softlayer.net`
private: `s3-api.us-geo.objectstorage.service.networklayer.com`
