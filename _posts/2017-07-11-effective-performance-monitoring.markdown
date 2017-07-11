---
title: "Effective Performance Monitoring"
layout: post
date: 2017-07-11 12:30
tag:
- best-practices
- performance
- monitoring

category: blog
comments: true
author: viktar
description: Effective Performance Monitoring
---

Close monitoring of your performance metrics is extremely critical for
ensuring that your application, integration services, and infrastructure
are available and performing well, so you can keep providing a great
customer experience successfully fast.
<br/><br/>
To have your performance metrics under your control I would recommend
to follow the best practises described below.

<!--more-->

{% include image.html name="measuring-performance-metrics.png"
           caption="Collect and analyze your performance metrics"
           alt="Collect and analyze your performance metrics"
           width="650" %}

## Collect All Performance Metrics
Monitoring and collecting different types of performance data
is useful for *current bottlenecks identifying* and for
[watching the trends](#follow-the-trends) of your application's
performance.<br/>

> *Collecting data is cheap, but not having it when you need it can be
expensive, so you should instrument everything, and collect all the
useful data you reasonably can. [Ilan Rabinovitch][5]*

There are following main areas you should closely keep your eye on:
* **Hardware resources**: CPU, memory, disc and network
* **JVM metrics**: GC activity and memory utilization
* **Business transactions**: execution time and throughput
* **Integration calls**: amount and response time
* **Caches utilization**: cache capacity and hits ratio
* **Errors**: types and amount of failures

Performance metrics are usually collected at a regular interval over the
time, e.g. *once per second* or *once per minute*. Depending on a
particular metric's nature **choose a sufficient granularity** for data
being collected.
<br/><br/>
And it's really important **to store all your collected data as long as
possible**.
It could be a concern to have all this data stored somewhere, however,
having collected data for *a year or more* makes it much easier to
understand long-time [trends](#follow-the-trends) and make forecasts for
the future.<br/>

## Follow the Trends
Having your application's
[performance metrics](#collect-all-performance-metrics) being collected
you have to keep watching the trend of your system's capacity. It will
give you an ability to analyze the current system’s performance and act
appropriately with trending improvements or degradations.<br/>

Also, it's important to have **automatic reports with weekly and monthly
trends**. Such reports could be generated with help of your APM tool,
such as [AppDynamics][3], [Datadog][6] or [Dynatrace][4].

## Define SLAs
The **Service Level Agreement** (SLA) establishes the metrics for
evaluating the system performance and provides the definitions for
availability and the scalability targets.<br/><br/>
SLA for all your [performance metrics](#collect-all-performance-metrics)
should be clearly defined and listed somewhere in your Wiki. As a result
you might have something like the following:

|Type | Metric | SLA |
|-----|--------|:-----:|
| Hardware | Application server CPU utilization | 70% |
| JVM | GC Major collections | 0 |
| Execution | Place Order transaction | 500 ms. |
| Integration | Data grid remote call | 10 ms. |
| Cache | Hit ratio | 0.8 |
| Error | HTTP 500 errors amount | 0 |


These defined SLAs will help you to react appropriately on performance
degradation in a real time.

## Setup Alerting
Having [SLA defined](#define-slas) for all your
[performance metrics](#collect-all-performance-metrics) it's required to
setup alert notifications for each particular case. With help of
[Nagios][1] and [Splunk][2] you can have all your systems metrics
monitored, and if any of them became out of defined SLA, you'll
receive an alerting email, so you can immediately react to the
situation.<br/><br/>
Alert messages should be *clear* and *meaningful* with a description of
possible cause of the occurred issue. Also, you might want them to have
a run-book with required actions to be immediately taken.

## Conclusion
Efficient monitoring of your performance metrics helps you to have a
detailed picture of your system's internal health. Keeping watching
system's performance trends makes it easy to plan your application's
scaling in accordance with business forecasts, such as holiday season
traffic amount.<br/>

Having **monitoring**, **reporting** and **alerting** properly
configured you're always able to answer to the following questions:
- *Is the application available and working fast as it's expected?*
- *What is the current throughput?*
- *Is it suffering some bottlenecks?*

[1]: https://www.nagios.org/
[2]: {{ site.url }}/blog/2017/06/29/performance-engineering-toolkit.html#splunk
[3]: https://www.appdynamics.com/
[4]: https://www.dynatrace.com/
[5]: https://www.linkedin.com/in/irabinovitch/
[6]: https://www.datadoghq.com
