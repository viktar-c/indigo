---
title: "Circuit Breaker Design Pattern"
layout: post
date: 2017-06-09 10:07
tag:
- design-pattern
- performance
- best-practices

category: blog
comments: true
author: viktar
description: Circuit Breaker Design Pattern Explaining with Implementation Example
---

Your overall application's performance might be strongly dependent on
how the third-party services you integrated with are performing.<br/><br/>
If some remote resource is currently experiencing performance troubles,
your calls to this service could be hanged out waiting for a response
**until a timeout is occurred**. Because of that issue the throughput
of your service will be drastically decreased, which definitely will
impact your end users' experience.
<br/><br/>
In this situation a **Circuit Breaker** mechanism could prevent your
system from such cascade performance impact.
<!--more-->

## Circuit Breaker Design Pattern Overview
The main idea here is to make your calls **fail-fast** if the remote
resource's calling ended up with a critical amount of failed attempts.
<br/>
In this case the **Circuit Breaker** mechanism will be switched to
_open_ state, which means **outage is in progress**. Since that
there is no sense to keep calling the problematic resource, and an
alternative flow could be used while outage is happening:

{% include image.html name="circuit-breaker-design-pattern-overview.png"
           caption="Circuit Breaker Design Pattern Overview"
           alt="Circuit Breaker Design Pattern Overview"
           width="650" %}

The **Circuit Breaker** will switch back to _closed_ state after
defined timeout. So, new attempts to reach out the resource will be
taken.
{% include image.html name="circuit-breaker-flow-diagram.png"
           caption="Circuit Breaker Design Pattern Flow Diagram"
           alt="Circuit Breaker Design Pattern Flow Diagram"
           width="650" %}

## Implementation Details
There is a simple implementation of the described **Circuit Breaker**
design pattern.
Basically, we're keeping track the following information about failed
attempts:
* **failure attempts' count** - to have an ability to check whether a
threshold is reached and the Circuit Breaker should be switched to
_open_ state.
* **a time when the first failed attempt occurred** - will help to maintain a
time frame for the failures threshold.
* **start time of the current outage** - a time when the Circuit Breaker
was switched to _open_ state. It will help to check how long the
current outage is happening.

The following **basic API** makes the usage of our
**Circuit Breaker** really simple and straightforward:
* `isOutageInProgress` - checks if the **Circuit Breaker** is in _open_
state
* `requestOutage` - requests the **Circuit Breaker** to be switched to
_open_ state

Here is an example with 5 failed calls **threshold** allowed
within a **one minute** time frame.
As soon as the **threshold** is reached, an alternative flow is
being processed for the next **5 minutes**:

```java
SimpleCircuitBreaker circuitBreaker = new SimpleCircuitBreaker(5, 300000L, 60000L);
if (!circuitBreaker.isOutageInProgress()) {
    try {
        // make a call to the resource

    } catch (RuntimeException e) {
        circuitBreaker.requestOutage();
        // process alternatives
    }
} else {
    // process alternatives
}
```

Full implementation of the [SimpleCircuitBreaker.java][2] could be found
in my [GitHub][1] repository.

## Summary
The **Circuit Breaker** mechanism could help to protect your
application from cascading failures and to provide a fallback behavior
for potentially failing calls to external resources.

[1]: https://github.com/viktar-charnarutski
[2]: https://github.com/viktar-charnarutski/best-practices/blob/master/designpatterns/src/concurency/SimpleCircuitBreaker.java

