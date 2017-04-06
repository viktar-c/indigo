---
title: "Coherence Federated Caching Configuration"
layout: post
date: 2017-04-05 15:53
tag:
- coherence
- synchronization
- cache

category: blog
comments: true
author: viktar
description: Configuration guide for Oracle Coherence Federated Caching
---

With the recent 12.2.1 release of [Oracle Coherence][1] it's became
possible to get your cached data synchronized across multiple data
centers using [Federated Caching][2] data grid functionality.<br/>
<br/>
Unfortunately, Oracle doesn't provide a clear documentation how to setup
the synchronization, so let's find out all required for proper data
replication across your data centers.

<!--more-->

### Step-by-step

1. [Before you start](#before-you-start)
2. [Disable local storage](#disable-local-storage)
3. [Switch to Proxy model](#switch-to-proxy-model)
4. [Change your cache type to Federated](#change-your-cache-type-to-federated)
5. [Define Federation participants](#define-federation-participants)
6. [Open required ACLs](#open-required-acls)

## Before you start
As it was already mentioned, the Federated Caching feature is available
only with **12.2.1** Coherence version, and if you're running on a one
of previous versions, for cache synchronization you migt want to go
with [Push Replication][3] open source solution of the
[Coherence Incubator][4], or consider [some workarounds][5] to get your
cache replicated.<br/>

## Disable local storage
If your application is a part of the Coherence cluster and it has local
storage enabled, be ready the application will talk directly to another
storage-enabled member on the other side. That means that it will be
participating in the synchronization process.<br/>
To avoid that I would recommend to disable local storage for your
application.

## Switch to Proxy model
Actually, if you decided to [disable local storage](#disable-local-storage-if-possible)
there is no much sense to have your application to be a part of the
Coherence cluster. So, you might want to run it as [Extend Client][6],
which is connected to [Proxy Service][7].<br/>
Here is a good [how-to manual][8] from Oracle how to do that.

## Change your cache type to Federated
All your cluster's participants (proxy and storage nodes) should have
defined **Federated Cache Scheme** in your overridden
`coherence-config.xml`. Nothing special, just follow steps mentioned in
[Defining Federated Cache Schemes][9]

## Define Federation participants
Your Coherence clusters from different data centers should be able
to discover each other and then establish a communication (synchronization)
process. For that purpose you have to specify **at least one storage node
from each cluster** in the participant's list in the
`tangosol-coherence-override.xml`:
{% highlight xml %}
<federation-config>
   <participants>
      <participant>
         <name>ClusterA</name>
         <name-service-addresses>
            <address>192.168.10.1</address>
            <port>8080</port>
         </name-service-addresses>
      </participant>
      <participant>
         <name>ClusterB</name>
         <name-service-addresses>
            <address>192.168.20.2</address>
            <port>8080</port>
         </name-service-addresses>
      </participant>
   </participants>
</federation-config>
{% endhighlight %}

This config file should be overridden for each cluster's member
(proxy and storage nodes).<br/>
For more details see [Defining Federation Participants][10]

## Open required ACLs
It might be not obvious, but apart from clusters' ports, specified in
[participants' list](#define-federation-participants), it's required to
open **localport** of each storage-enabled member from both sides.<br/>
Basically, **cluster ports** are required for discover purpose, and
**localports** - for synchronization process.

{% include image.html name="coherence-federated-caching.png"
           caption="Coherence Federated Caching"
           alt="Coherence Federated Caching"
           width="650" %}
* **8070** - proxy ports (*ACLs are not required*)
* **8080** - cluster ports (**ACLs are required**)
* **8090** - localports (**ACLs are required**)

[1]: https://docs.oracle.com/middleware/1221/coherence/index.html
[2]: https://docs.oracle.com/middleware/1221/coherence/administer/replication.htm#COHAG5388
[3]: http://coherence-community.github.io/coherence-incubator/11.0.0/pushreplicationpattern/index.html
[4]: http://coherence-community.github.io/coherence-incubator/11.0.0/
[5]: {% post_url 2017-04-02-data-synchronization-across-data-centers %}
[6]: https://docs.oracle.com/cd/E18686_01/coh.37/e18680/coherenceextend.htm#COHGS213
[7]: https://docs.oracle.com/cd/E18686_01/coh.37/e18680/coherenceextend.htm#COHGS215
[8]: https://docs.oracle.com/cd/E15357_01/coh.360/e15726/gs_example.htm#COHCG4921
[9]: https://docs.oracle.com/middleware/1221/coherence/administer/replication.htm#COHAG5398
[10]: https://docs.oracle.com/middleware/1221/coherence/administer/replication.htm#COHAG5395
