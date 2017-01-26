---
title: Creating and using buckets
keywords: 
last_updated: November 18, 2016
tags: 
summary: 
sidebar: crs_sidebar
permalink: crs-control-buckets.html
folder: crs
toc: false
---

{% include note.html content="The user interface portal provides a high level view of a storage account.  It is possible to create buckets and upload objects using the portal, but typically most interaction with the object store is done through the API by a client application." %}

#### Using buckets
1. Click the Account Name in the above accounts list to open the details page for that account.
2. The details page shows the following info on the top header bar:
   * Name of the account currently being viewed
   * The current region being viewed
   * The total capacity usage in the current billing cycle
   * The total bandwidth usage in the current billing cycle
3. The page also shows the list of buckets present in the account.
4. A new bucket can be added by clicking on the **+** button at the right of the first row.
5. The new bucket name can be entered and the new bucket added, by clicking the **Add** button. Bucket names must be DNS-compliant. Names must be between 3 and 63 characters long, must be made of lowercase letters, numbers, and dashes, must be globally unique, and cannot appear to be an IP address. A common approach to ensuring uniqueness is to append a UUID or other distinctive suffix to bucket names. 
6. A bucket can be deleted, by clicking the red **-** button to the extreme right of the bucket name. This opens a confirmation window.
7. **Note: only empty buckets can be deleted**.  Attempting to delete a non-empty bucket will result in an error.
7. Click the **Delete** button to remove the bucket from the account. 


 

