---
title: "Bottlenecks of Coherence Datagrid Setup"
layout: post
date: 2017-07-30 12:30
tag:
- best-practices
- performance
- in-memory computing

category: blog
comments: true
author: viktar
description: Checkup for Performance Bottlenecks of Coherence datagrid setup with help of JMeter and JUnit
---

Load testing of your application on a regular basis is a right
way to keep an eye on the app's performance trends with all configuration
and functional changes in place.
<br/>
However, one performance bottleneck could be hiding after another one,
and you won't know about the issue until the current limitations are fixed.
<br/><br/>
That means that it might be possible that you're not able to get your
Coherence integration properly loaded if the overall application's
performance is struggling with a slowness caused by, let's say, poor
performance of your DB. So, it's required to find a way how to check the
integration for potential bottlenecks in an isolated manner having all
calls made directly to the grid.

<!--more-->

{% include image.html name="post-title-image.png" width="650" %}
## Potential Bottlenecks in Oracle Coherence Setup
There are few potential bottlenecks in Oracle Coherence setup which are
very common and sometimes not very obvious.
Let's see what should be double-checked from the configuration point
of view before your GOLIVE date:

1. **Inefficient hardware resources** could cause some failures within
your [SLAs][4] occurring. In case of such failure, Coherence needs to
perform more processing (i.e. the recovery and rebalancing of lost data)
with fewer resources.

2. **Improper JVM configuration** could lead to long
Garbage Collection pauses, low Heap Space or even Out Of Memory failures.

3. **Insufficient processing resources**, such as [static thread pooling][6],
could cause the task backlog queue growing and affecting the application
performance, throughput, and scalability.

{% include image.html name="insufficient-processing-resources.png"
           caption="Growing Task Backlog as a result of insufficient processing resources"
           alt="Growing Task Backlog as a result of insufficient processing resources"
           width="650" %}

## Testing Approach Overview
Direct and isolated performance testing of the Coherence datagrid means
that the application itself shouldn't be involved into the test.
<br/>It's required to have a standalone client which should be able to
fire ```get``` and ```put``` calls against your distributed caches.
<br/>
Despite the client is not dependent on the application logic, the client
should be able to manipulate with exact copies of the serializable data
being initialized by the application. Objects passed to the datagrid
should have the same structure, size, and serialization approach.

## Solution Details
Fortunately, there is a really easy way to get such test client
implemented - your JUnit tests could be used as a code base for all ```get```
and ```put``` operations.<br/>
Hope, your code is fully covered with all required unit tests, otherwise
you have to create some tests for basic datagrid calls. Something like:

```java
@org.junit.Test
public void put() throws Exception {
    String exMessage = null;
    try {
        Order order = order();
        cache.put(order.profileId(), order);
    } catch (Exception ex) {
        exMessage = ex.getMessage();
    }
    assertNull(String.format("put failed due to %s", exMessage), exMessage);
}

@org.junit.Test
public void get() throws Exception {
    Order result = (Order) cache.get(cachedProfileId());
    assertNotNull(String.format("Nothing is returned for %s key.", profileId), result);
}
```

When you're got ready with JUnit tests, get this code packed in a jar and
along with the coherence lib get it placed under your JMeter's
```lib/junit``` directory. Then specify these libs as a part of JMeter's
classpath:

{% include image.html name="jmeter-classpath.png"
           caption="Place your JUnit tests on JMeter's classpath"
           alt="Place your JUnit tests on JMeter's classpath"
           width="650" %}

JMeter's [JUnit Request][5] samplers allows you to compose a test
scenario using your junit tests. Coherence connection should be initialized
before the test scenario, which is a sequence of ```get``` and ```put```
calls wrapped in junit tests:

{% include image.html name="jmeter-scenario.png"
           caption="JMeter scenario of JUnit tests"
           alt="JMeter scenario of JUnit tests"
           width="650" %}

Also, it's important to have your coherence integration settings
available for JMeter test agents, so appropriate parameters should be
used in your test's start-up script:

```
./jmeter.sh -n -t coherence-test.jmx -l
../results/coherence-test.jtl -e -o
../results/coherence-test
-Dtangosol.coherence.cacheconfig=coherence-test-config.xml
```

## Conclusion
Series of load tests for different Coherence configuration setups
will help you to define system settings for optimal performance of the
Coherence datagrid integration.
<br/>
Also, such JMeter + JUnit bundle could allow you to start with performance
analysis from the very beginning of the application development.

## Sources
Full implementation of the Coherence JUnit tests could be found
in the [coherence-write-behind][7] project of my [GitHub][8] repository.

[1]: http://jmeter.apache.org/
[2]: http://junit.org/junit4/
[3]: http://www.oracle.com/pls/topic/lookup?ctx=fmwlatest&id=COHAG
[4]: {% post_url 2017-07-11-effective-performance-monitoring %}#define-slas
[5]: http://jmeter.apache.org/usermanual/junitsampler_tutorial.html
[6]: https://docs.oracle.com/middleware/1212/coherence/COHCG/gs_best.htm#COHCG5296
[7]: https://github.com/viktar-charnarutski/coherence-write-behind
[8]: https://github.com/viktar-charnarutski
