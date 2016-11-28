---
title: Creating a new customer account
keywords: 
last_updated: November 18, 2016
tags: 
summary: IBM Cloud is unifiying PaaS and IaaS under Bluemix, and there are  multiple ways to create accounts that can order IBM COS.
sidebar: crs_sidebar
permalink: crs-create-account.html
folder: crs
---

## Understanding IBM Cloud platform
 
Until recently, IBM offered Platform-as-a-Service and Infrastructure-as-a-Service in two distinct environements: [Bluemix](https://bluemix.net){: new_window} and [SoftLayer](http://www.softlayer.com){: new_window}.  Bluemix provides a robust application development platform with direct access to cutting edge IBM technologies and DevOps services. SoftLayer provides access to infrastructure services, such as bare metal or virtual servers, data storage, and networking.

Now these lines are dissolving as we integrate the SoftLayer infrastructure offerings into the [Bluemix catalog of services](https://console.ng.bluemix.net/catalog/){: new_window}. Existing SoftLayer customers are encouraged to take advantage of [using an IBMid for single-sign-on authentication](http://blog.softlayer.com/2016/new-softlayer-accounts-now-ibmid-authentication){: new_window}, and it is possible to [link existing SoftLayer and Bluemix accounts](http://blog.softlayer.com/2016/meet-integrated-ibm-cloud-platform-softlayer-and-bluemix){: new_window}. As of October 2016, SoftLayer is transitioning to the new name Bluemix Infrastructure, and computing, storage, networking services are provided through both the Bluemix catalog and (for a time) the SoftLayer website. 

{% include important.html content="In order to distinguish it from the existing OpenStack Swift based object storage offerings, 'IBM COS Cross-Region' is commonly referred to by the name 'Cloud Object Storage - S3 API' in order forms.  'IBM COS Cross-Region' and 'COS - S3 API' should be considered interchangeable terms within IBM Cloud at this time, although this is subject to change in accordance with future COS offerings." %}

## Creating a new Bluemix account

Before ordering a new IBM COS Cross-Region storage account, it is necessary to create a customer account first.

1. Go to [bluemix.net](https://bluemix.net){: new_window} and click on the **Create a Free Account** button.
2. Fill out the form, providing your email address, name, region, and phone number.  Choose a password.
3. Follow the link provided by the confirmation email, and follow the links to log in to Bluemix.
4. Before using Bluemix, you need to set up a basic development environment.  Choose a region based on your geographic location, and choose a name for your organization.  Don't worry about being perfect - you can change it or create new organizations later. Follow the **Create** link.
5. Now you need to name your first development space.  Again, this can be changed at any time.  After you've chosen a name, follow the **Create** link.
6. Follow the **I'm Ready** link to get started with Bluemix.
7. Ordering infrastructure services requires upgrading to a Pay-As-You-Go account.  Upgraded accounts still recieve runtime and service allowances, and are only charged for resources used.  Go to the Bluemix catalog, and follow the link for **Cloud Object Storage - S3 API**.
8. Follow the **Upgrade** link.
9. Fill out the Contact and Billing information forms, check the box to acknowledge Cloud Service terms, and follow the **Submit Order** link.
10. Follow the **Done** link, and wait for an email confirming your account upgrade.
7. Next up is ordering a new object storage account!

## Creating a new SoftLayer account

{% include note.html content="Bluemix Infrastructure will be the primary platform for ordering storage in the future and new customers are encouraged to begin with Bluemix, although it is still possible to create an account directly through SoftLayer." %}

1. Got to [softlayer.com](http://www.softlayer.com) and choose **Products & Services** > **Object Storage** from the top menu.
2. Follow the **Order Now** link.
3. Choose **Cloud Object Storage - S3 API** and then follow the **Add to Order** link. 
4. Fill out the Contact and Billing information forms, check the box to acknowledge Cloud Service terms, and follow the **Submit Order** link. Note that the location indicated in the order form is unrelated to the location or endpoint used to store or connect to your data.
5. You will recieve an email indicated that the order is under review. This may take some time.







