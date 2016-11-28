---
title: Endpoints
keywords: 
last_updated: November 18, 2016
tags: 
summary: 
sidebar: crs_sidebar
permalink: crs-endpoints.html
folder: crs
---

SoftLayer provides regional endpoints for connecting applications or clients that use the S3 API to the public cloud. Endpoints should be chosen based on proximity to the application servers accessing the object store to minimize latency.  For workloads not concentrated in a single geographic area, the US Region endpoint distributes connections among the regional data centers.  Endpoints can be accessed over either plaintext or SSL depending on security requirements.

Types of endpoint:

* **Public endpoints** can be accessed from anywhere and charges are assessed on outgoing bandwidth. Incoming bandwidth is free. Public endpoints should be used for access not originating from a SoftLayer data center. 
* **Private endpoints** can be accessed by customers running Virtual Machines or Bare Metal Servers on SoftLayer. Private endpoints do not incur charges for any outgoing or incoming bandwidth even if the traffic is cross regions or across data centers. Customers with workloads running in a SoftLayer data center in Dallas, San Jose, or Washington should use the endpoint for that same city. Customers with existing workloads running in a remote SoftLayer data center should use the US Region endpoint.
* **Account Defined Network endpoints** allow customers to bring their own IP addresses. If a customer has an ADN account and uses an ADN endpoint, they will not be charged for any outgoing or incoming bandwidth. ADN endpoints only appear in the control console in ADN accounts. If you are interested in learning more about ADN accounts, contact customer support.

{% include important.html content="Bluemix infrastructure offerings are connected to a three-tiered network, segmenting public, private, and management traffic. Infrastructure on a customer's Bluemix account may transfer data between one another across the private network at no cost. Infrastructure offerings (such as bare metal servers, virtual servers, and cloud storage) connect to other applications and services in the Bluemix catalog (such as Watson services, containers, runtimes) across the public network, so data transfer between those two types of offerings is  metered and charged at standard public network bandwidth rates." %}


<table>
  <thead>
    <tr>
      <th>Region</th>
      <th>Type</th>
      <th>Endpoint</th>
    </tr>
  </thead>
    <tr>
    <td rowspan="3">US Region</td>
    <td>public</td>
    <td><code class="highlighter-rouge">s3-api.us-geo.objectstorage.softlayer.net</code></td>
  </tr>
  <tr>
    <td>private</td>
    <td><code class="highlighter-rouge">s3-api.us-geo.objectstorage.service.networklayer.com</code></td>
  </tr>
  <tr>
    <td>adn</td>
    <td><code class="highlighter-rouge">s3-api.us-geo.objectstorage.adn.networklayer.com</code></td>
  </tr>
  <tr>
    <td rowspan="3">Dallas</td>
    <td>public</td>
    <td><code class="highlighter-rouge">s3-api.dal-us-geo.objectstorage.softlayer.net</code></td>
  </tr>
  <tr>
    <td>private</td>
    <td><code class="highlighter-rouge">s3-api.dal-us-geo.objectstorage.service.networklayer.com</code></td>
  </tr>
  <tr>
    <td>adn</td>
    <td><code class="highlighter-rouge">s3-api.dal-us-geo.objectstorage.adn.networklayer.com</code></td>
  </tr>
  <tr>
    <td rowspan="3">San Jose</td>
        <td>public</td>
    <td><code class="highlighter-rouge">s3-api.sjc-us-geo.objectstorage.softlayer.net</code></td>
  </tr>
  <tr>
    <td>private</td>
    <td><code class="highlighter-rouge">s3-api.sjc-us-geo.objectstorage.service.networklayer.com</code></td>
  </tr>
  <tr>
    <td>adn</td>
    <td><code class="highlighter-rouge">s3-api.sjc-us-geo.objectstorage.adn.networklayer.com</code></td>
  </tr>
  <tr>
    <td rowspan="3">Washington</td>
    <td>public</td>
    <td><code class="highlighter-rouge">s3-api.wdc-us-geo.objectstorage.softlayer.net</code></td>
  </tr>
  <tr>
    <td>private</td>
    <td><code class="highlighter-rouge">s3-api.wdc-us-geo.objectstorage.service.networklayer.com</code></td>
  </tr>
  <tr>
    <td>adn</td>
    <td><code class="highlighter-rouge">s3-api.wdc-us-geo.objectstorage.adn.networklayer.com</code></td>
  </tr>
</table>
{:.endpointtable}