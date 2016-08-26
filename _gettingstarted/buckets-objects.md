---
layout: page
title:  "Managing Buckets & Objects"
featured: true
weight: 3
tags: []
author: Nick
dateAdded: August 18th, 2016
---

```** INTERNAL DRAFT NOTE: Screenshots to be updated prior to publishing **```

The user interface portal provides a high level view of a storage account.  It is possible to create buckets and upload objects using the portal, but typically most interaction with the object store is done through the API by a client application. 

#### Using buckets
1. Clicking on the Account Name in the above accounts list will open the details page for that account.
    ![Buckets]({{ site.baseurl }}/img/bu1.png)
2. The details page shows the following info on the top Header bar.
    * Name of the account currently being viewed. 
    * The current region being viewed.
    * The total capacity usage in the current billing cycle.
    * The total bandwidth usage in the current billing cycle.
3. The page also shows the list of buckets present in the account.
4. A new bucket can be added by clicking on the ‘+’ button at the right of the first row.
    ![Buckets]({{ site.baseurl }}/img/bu2.png)
5. The new bucket name (it must be a DNS-compliant, globally unique name) can be entered and the new bucket added by clicking on the ‘Add’ button.
6. A bucket can be deleted by clicking on the red ‘-’ button to the extreme right of the bucket name. This opens a confirmation window.
    ![Buckets]({{ site.baseurl }}/img/bu3.png)
7. Clicking the ‘Delete’ button removes the bucket from the account. **Note: only empty buckets can be deleted**.  Attempting to delete a non-empty bucket will result in an error.

#### Using objects

1. The objects present within a bucket can be viewed by clicking on the bucket name on the buckets list.
    ![Objects]({{ site.baseurl }}/img/obj1.png)
2. The header would be updated to include the following info.
    * Name of the account currently being viewed.
    * The current region being viewed.
    * The name of the bucket whose objects are currently displayed.
    * The Type of the bucket whose objects are currently displayed.
3. Above the grid on the left is a link with the name of the bucket currently viewed. Clicking on the link will show the list of buckets again.
4. The grid shows the list of objects in the bucket.
5. A new object can be added by clicking on the ‘+’ button at the right of the first row. 
    ![Objects]({{ site.baseurl }}/img/obj2.png)
6. The file can be selected from the file system by clicking on the select button and the new file uploaded by clicking on the ‘Add’ button.  When using the portal to add an object, file size is limited to 20 megabytes.
7. An object can be deleted by clicking on the red ‘-’ button to the extreme right of the object name. This opens a confirmation window.
    ![Objects]({{ site.baseurl }}/img/obj3.png)
8. Clicking the ‘Delete’ button removes the object from the bucket.