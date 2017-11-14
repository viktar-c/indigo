---
title: "Introducing Computing Into Your Data Grid"
layout: post
date: 2017-11-13 17:44

tag:
- best-practices
- performance
- in-memory computing

keywords:
- distributed cache
- oracle coherence
- coherence data grid
- coherence cache
- in-memory computing

category: blog
comments: true
author: viktar
description: Introducing computing into your Data Grid
---

It might be possible that your data grid's potential is being utilized
very insignificantly.
Having it efficiently running as a *distributed cache*, the most intelligent
features are staying idle, and your data grid actually is running in a
*passive mode*.<br/>
Introducing a **simple computing** into your data grid could be really simple
and extremely beneficial from performance and functional perspectives.

<!--more-->

{% include image.html name="introducing-computing.jpg" width="650" %}
## Time To Live
It's very important to keep the cached data up-to-date, so here is an
important question to answer:
> *How long your cached object is supposed to live in the data grid?*

Obviously, it depends on your business case, and it's being specified
in your data grid configuration:
```xml
<internal-cache-scheme>
    <local-scheme>
        <expiry-delay>1m</expiry-delay>
    </local-scheme>
</internal-cache-scheme>
```
This specified *Time To Live* (TTL) value means that all items stored in this
distributed cache will be cached
for 1 minute. After that time it will be evicted, and the most recent
version of the data should be loaded to the data grid on demand.
<br/><br/>
And it works fine until you realised that some part of your data is
not being updated frequently. **In 1 minute exactly the same version
of an item could be reloaded.** Which is obviously waisting of your
server's resources.
<br/><br/>
To get it fixed (*or at least improved*) you might want to specify a
custom TTL for every single cached object, depending on some proven
business rules.

## Business Rules
You supposed to make a detailed research and to come up with business rules
to be applied to your custom TTL computing. For example, having inventory stock
information you can check how fast a most popular product could be sold
out. And as a result defined threshold values could be used in your
business rules' implementation. E.g:
```
TTL = STOCK LEVEL < THRESHOLD ? DEFAULT TTL : STOCK LEVEL / THRESHOLD * DEFAULT TTL
```

For default TTL you can use your configured ```expiry-delay``` from
data grid configuration settings.

## Implementation
With Oracle Coherence data grid introducing of a custom computing could
be as simple as [overriding the backing map schema][1]'s implementation:

```java
package com.viktarx;

public class MyInventoryReadWriteCacheMap extends ReadWriteBackingMap {

    // parent constructors are ommited

    @Override
    protected long extractExpiry(Entry entry) {
        return ((InventoryPofObject) entry.getValue()).stockLevel()
                < THRESHOLD
                ? super.extractExpiry(entry)
                : stockLevel / THRESHOLD * super.extractExpiry(entry);
    }
}
```

A reference to this custom backing map class should be specified in the
xml configuration of the cache:

```xml
<backing-map-scheme>
    <read-write-backing-map-scheme>
        <class-name>com.viktarx.MyInventoryReadWriteCacheMap</class-name>
        <internal-cache-scheme>
            <local-scheme>
                <expiry-delay>1m</expiry-delay>
            </local-scheme>
        </internal-cache-scheme>
		<!-- remain configuration is ommited -->
    </read-write-backing-map-scheme>
</backing-map-scheme>
```

And that's it - now your data grid will calculate a custom TTL for every
newly cached item before putting it to the backing map storage.

## Some Limitations
Unfortunately, in case you're using **Oracle Coherence 12.2** version,
there is no easy way to provide additional configuration to make your
business rules more flexible.<br/>
Per [Oracle's documentation][2] it could be done with ```<init-param>``` tag
in your cache's configuration description. However, it seems it's not
supported any more.

## Efficiency and Benefits
There are few points you have to keep in mind with introducing a custom
computation into your distributed data grid:
1. **Computing operations should be blazing fast.** <br/>
Otherwise, you might introduce a performance degradation to loading the
data functionality.
2. **Your cached data should be actual and relevant.** <br/>
Playing around custom cache settings, such as computing
of ```expiry-delay``` value, could affect your end-users' experience if
the cached data got critically outdated.

## Summary
With introducing a computing into your data grid you can get
your application offloaded from some routine computation.<br/>
Also, it could decouple some application's functionality
turning your passive data storage into a powerful data-processing
service.

[1]: https://docs.oracle.com/middleware/1212/coherence/COHDG/cache_back.htm#COHDG5175
[2]: https://docs.oracle.com/middleware/1212/coherence/COHDG/appendix_cacheconfig.htm#COHDG4764
