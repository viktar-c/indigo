---
title: "Data Synchronization Across Data Centers"
layout: post
date: 2017-04-02 22:30
tag:
- data
- synchronization
- cache
category: blog
comments: true
author: viktar
description: Several options to get your data synchronized across data centers in a real time
---

If you have your application running in different data centers in
<b>active-active manner</b> (<i>traffic is served by all data centers
simultaneously</i>), you might want them to use the same datasource for
up-to-date data processing.<br/>
In-memory data grid could be a good solution for sharing information
across multiple data centers in a real time.<br/>
And there are several ways how to build up such synchronization.
<!--more-->

{% include image.html name="application-running-in-separate-dcs.png"
           caption="Application Running in Separate Data Centers"
           alt="Application Running in Separate Data Centers"
           width="650" %}

## Two separate In-Memory Data Grids running in synchronized mode
All most popular In-Memory Data Grid applications support data
replication over Wide Area Network (WAN).

{% include image.html name="imdg-for-datacenters-sync-solution.png"
           caption="In-Memory Data Grid Synchronization Across Data Centers"
           alt="In-Memory Data Grid Synchronization Across Data Centers"
           width="650" %}

Such data synchronization model is preferable because of following pross:
<ul>
    <li> each data center has it's own independent architecture </li>
    <li> maximum bandwidth is configurable </li>
    <li> limited number of ACL to be opened between data centers </li>
    <li> communication between clusters could be easily encrypted </li>
</ul>

---

## Single In-Memory Data Grid
If, for some reason, there is no ability to spin up a separate IMDG
cluster in each of your data centers, then you can think about to have
your application connected directly to the data grid running in one of
the data centers:

{% include image.html name="single-imdg.png"
           caption="Single In-Memory Data Grid"
           alt="Single In-Memory Data Grid"
           width="650" %}

However, such communication model has significant drawbacks:
<ul>
    <li> strong dependency on data center which has running shared data
    grid </li>
    <li> network bandwidth could be overflowed if you have a lot of
    applications got connected to the shared data grid </li>
    <li> number of ACL to be opened between data centers depends on
    applications' amount you'd like to have connected
         to the shared data grid</li>
    <li> all communication with IMDG should be configured via proxy
    server running in-front of data grid </li>
    <li> custom solution for messages' encryption could be required </li>
</ul>

---

## Shared In-Memory Data Grid
It's strongly not recommended to spread your data grid cluster across
multiple data centers. Anyway, due to product licence restrictions you
might want to have your storage nodes from one data center being
communicated with storage nodes from another data center forming one
single cluster:

{% include image.html name="shared-imdg.png"
           caption="Shared In-Memory Data Grid"
           alt="Shared In-Memory Data Grid"
           width="650" %}

This model could be very unstable because of network latency, and
cluster's communication between nodes from separate data centers could
be interrupting.
Also, inter-nodes communication is not secure, so your data could be
intercepted and compromised.
