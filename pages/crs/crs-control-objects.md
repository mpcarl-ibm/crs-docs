---
title: Creating and using objects  
keywords: 
last_updated: November 18, 2016
tags: 
summary: 
sidebar: crs_sidebar
permalink: crs-control-objects.html
folder: crs
toc: false
---
{% include note.html content="The user interface portal provides a high level view of a storage account.  It is possible to create buckets and upload objects using the portal, but typically most interaction with the object store is done through the API by a client application." %}

#### Using objects
1. The objects present within a bucket can be viewed, by clicking the bucket name on the buckets list.
2. The header is updated to include the following info:
  * Name of the account currently being viewed
  * The current region being viewed
  * The name of the bucket whose objects are currently displayed
  * The type of the bucket whose objects are currently displayed
3. Above the grid on the left is a link with the name of the bucket currently viewed. Click the link to show the list of buckets again.
4. The grid shows the list of objects in the bucket.
5. A new object can be added, by clicking the **+** button at the right of the first row. 
6. The file can be selected from the file system, by clicking the **select** button and the new file uploaded, by clicking the **Add** button.  When using the portal to add an object, file size is limited to 20 megabytes. (When using the API, objects as large as 5 gigabytes can be uploaded in a single `PUT`, and as large as 5 terabytes when using a multi-part upload.)
7. An object can be deleted, by clicking on the red **-** button to the extreme right of the object name. This opens a confirmation window.
8. Click the **Delete** button to remove the object from the bucket.