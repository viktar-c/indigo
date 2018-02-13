---
title: "Memory Usage Optimization for Distributed Cache Cluster"
layout: post
date: 2018-02-13 11:05

tag:
- best-practices
- memory optimization
- in-memory computing

keywords:
- memory optimization
- distributed cache
- oracle coherence
- coherence data grid
- coherence cache
- in-memory computing

category: blog
comments: true
author: viktar
description: Memory Usage Optimization for Distributed Cache Cluster
---

You might find your distributed cache's cluster hosting different types
of caches: _write-behind_, _read-through_ and _cache-aside_ caches.
The more your storage nodes hosts caches, the more memory it requires.
So, let's see what optimizations tweaks could be applied to allow your
caches feel well and safe.

<!--more-->

{% include image.html name="coherence-cluster-memory-usage-optimization.png" width="650" %}

## Count of Backups
In the most common use cases a cache is mapped to the [DistributedCache][1] service:
> _A distributed, or partitioned, cache is a clustered, fault-tolerant cache that has linear scalability._
> _Data is partitioned among all the machines of the cluster._

Apart of holding a part of data, by default this service assumes
storing backups from all other storage nodes. That means that having 3
storage nodes each of them is hosting ⅓ of data, and 2 backups from
2 other nodes (⅓ + ⅓).
<br/>
So, **if you're caching non-critical data, and it's acceptable to miss
some piece of cached data**, you can go ahead and disable backups holding
for this particular service:

```xml
<distributed-scheme>
    <scheme-name>products</scheme-name>
    <service-name>DistributedCache</service-name>
    ...
    <backup-count>0</backup-count>
</distributed-scheme>
```

In this case you can assume your cache's capacity boosted up to N times
(_N - is a number of storage nodes in the cluster_) since it's not tacking
care of backups storing any more.
<br/>
Just keep in mind that it's not allowed to run different caches with
different number of backups mapped to the same service. That means that
all the caches mapped to the [DistributedCache][1] service should have
either enabled or disabled data backups.

## Thresholds Defining
It's supposed to be a good practice to measure your storage node's capacity
and define thresholds for all the caches hosted by the node.
<br/>
You can define a right amount of items allowed to be cached at the same
time:

```xml
<distributed-scheme>
    <scheme-name>products</scheme-name>
    <service-name>DistributedCache</service-name>
    <backing-map-scheme>
        <local-scheme>
            <eviction-policy>LRU</eviction-policy>
            <high-units>10000</high-units>
            <expiry-delay>0</expiry-delay>
            <unit-calculator>FIXED</unit-calculator>
        </local-scheme>
    </backing-map-scheme>
    ...
</distributed-scheme>
```

Here you have to define an average object's size and come up with total
amount calculation. E.g. having 100KB per object and 1GB of heap memory,
you can allow 10K objects to be cached.
<br/>

Or you can limit your cache size by consumed memory:

```xml
<distributed-scheme>
    <scheme-name>products</scheme-name>
    <service-name>DistributedCache</service-name>
    <backing-map-scheme>
        <local-scheme>
            <eviction-policy>LRU</eviction-policy>
            <high-units>1000000</high-units>
            <expiry-delay>0</expiry-delay>
            <unit-calculator>BINARY</unit-calculator>
        </local-scheme>
    </backing-map-scheme>
    ...
</distributed-scheme>
```

So now your _products_ cache won't grow beyond 1GB, and because of defined
[Least Recently Used][2] eviction policy, old cached items will be
replaced with new ones.

## Summary
As you can see, it's very important to take care about your cache's
capacity in advance and have it protected from overpopulation.
So, you'll have your distributed cache's cluster very unlikely
fails because of out of memory.

[1]: https://docs.oracle.com/cd/E14526_01/coh.350/e14509/appcachetypes.htm#COHDG320
[2]: https://en.wikipedia.org/wiki/Cache_replacement_policies#Least_Recently_Used_(LRU)
