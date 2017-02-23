---
title: Generating Service credentials
keywords: 
tags:
sidebar: bx_sidebar
permalink: bx-credentials
summary: 
toc: false
---

Service credentials are used to provide authentication and security for your storage instance. You can create new credentials in the UI or by using the S3 API.
{: shortdesc}

The API uses the authentication information from the environment variables in the following table.
<table>
<caption> Table 1. Authentication variables and descriptions </caption>
  <tr>
    <th> Environment variable </th>
    <th> Description </th>
  </tr>
  <tr>
    <td> Endpoint URL </td>
    <td> The URL that is used to access your stored objects. </td>
  </tr>
  <tr>
    <td> Access Key ID </td>
    <td> Your key to access the S3 API. </td>
  </tr>
  <tr>
    <td> Secret Access Key </td>
    <td> Your security credentials. </td>
  </tr>
  <tr>
    <td> Region </td>
    <td> The region in which your objects are stored. </td>
  </tr>
</table>


#### To add credentials in the UI:

1. In your service instance dashboard, click the **Service Credentials** tab.
2. Click **New Credential**.
3. Provide a name for the credential.
4. Click **Add**.
