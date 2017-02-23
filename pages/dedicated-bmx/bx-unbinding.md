---
title: Unbinding and deprovisioning
keywords: 
tags:
sidebar: bx_sidebar
permalink: bx-unbinding
summary: 
toc: false
---

You can unbind or deprovision an instance of Object Storage for  that you no longer need.
{: shortdesc}

## Unbinding your instance

If you no longer have a need for object storage in your Cloud Foundry app, but you want to maintain your saved data, you can unbind your instance of the service from your app.

**Attention:** If you unbind an object storage instance from a Bluemix application, or delete the service key, all of your credentials for that instance are deleted and cannot be restored.

1. To see the services bound to your app, click the connections tab in your Cloud Foundry application.
2. Locate the service you that you want to unbind and click the menu button on the service tile.
3. Select **Unbind service**.
4. Clear the **Delete service instance** box and click **OK**.
5. Click **Restage** for your change to take effect.



## Deprovisioning your instance

If you no longer have a need for object storage you can deprovision your service instance.

1. From your service instance, delete all buckets and objects. You can use the multi-delete option.
2. Select the **Delete service** option and click **Delete** when prompted to verify.



#### To add credentials in the UI:

1. In your service instance dashboard, click the **Service Credentials** tab.
2. Click **New Credential**.
3. Provide a name for the credential.
4. Click **Add**.
