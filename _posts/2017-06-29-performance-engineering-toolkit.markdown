---
title: "Performance Engineering Toolkit"
layout: post
date: 2017-06-29 14:50
tag:
- tools
- performance

category: blog
comments: true
author: viktar
description: Performance Engineering Toolkit
---

Engineers are supposed to have various tools to have their routine work
done in the most convenient and efficient way. And the wider expertise
area the engineer has, the bigger and more diverse his toolkit is.
<br/><br/>
Performance engineering requires quite specific and dedicated
skills such as: systems architecture and engineering, troubleshooting
and analysis, load modeling and testing. And, to apply these skills
with high efficiency, performance engineer has to be familiar with
different and sophisticated set of tools.<br/><br/>
Here is my set of tools with brief functionality description:

<!--more-->

* [JetBrains IntelliJ IDEA](#jetbrains-intellij-idea)
* [Java VisualVM](#java-visualvm)
* [YourKit Java Profiler](#yourkit-java-profiler)
* [Eclipse MAT Analyser](#eclipse-mat-analyser)
* [IBM Thread Analyzer](#ibm-thread-analyzer)
* [GCeasy](#gceasy)
* [Apache JMeter](#apache-jmeter)
* [Splunk](#splunk)

## JetBrains IntelliJ IDEA
Being a software developer you definitely know the importance of using
a world-class integrated development environment (IDE).<br/><br/>
There are a lot of disputes which IDE is better: [IntelliJ][1] or
[Eclipse][5], and someone can say that these products are equal,
and your choice is a matter of taste. Wrong! Recommend him to switch
to the [IntelliJ][1] due to the following objective reasons:
* intelligent [debugging][6]
* intelligent [refactoring][7]
* intelligent [autocomplete][8]

{% include image.html name="jetbrains-intellij-idea.png"
           caption="JetBrains IntelliJ IDEA as the most intelligent IDE"
           alt="JetBrains IntelliJ IDEA as the most intelligent IDE"
           width="650" %}

[IntelliJ][1] literally knows all about your code. It indexes all
the project's files, analyses and understands it. So, sometimes you
could be really surprised how smart your IDE is.<br/>
There are a lot of plugins, and configurable settings for
[IntelliJ IDEA][1], which makes your experience unique.<br/>
Check out my current <a href="/assets/tools/settings/intellij.jar">
IntelliJ IDEA settings</a>.

## Java VisualVM
[Java Virtual Machine][2] is a monitoring, troubleshooting, and profiling
tool which is bundled with JDK version 6.7 or later. You just need to
[enable][3] your JVM (local or remote) for remote monitoring] and connect
to the JVM process with the [jvisualvm][2] tool. That's it! Now you're
able to:
* generate and analyse heap dumps
* generate and analyse thread dumps
* monitor and ask for garbage collection
* track down memory leaks
* check the current CPU and memory utilization

{% include image.html name="java-visualvm.png"
           caption="Java Virtual Machine monitoring, troubleshooting,
           and profiling tool"
           alt="Java Virtual Machine monitoring, troubleshooting, and
           profiling tool"
           width="650" %}

Another great thing about the [VisualVM][2] tool is an ability to expand
the basic functionality with pluggable extensions. You can choose an
already existed plugin from the [Java VisualVM catalog][13] or create
your own extension depending on your needs, something like
[Coherence JVisualVM Plug-In][4].

## YourKit Java Profiler
Sometimes you might need more comprehensive and intelligent Java code
profiling than [Java VisualVM](#java-visualvm) could provide. With
[YourKit Java Profiler][12] you can allow the following features not
available in JDK built-in tools:

* IDE integration
* CPU profiling
* Database statement analysis
* Memory and memory leak analysis
* Remote application profiling

{% include image.html name="yourkit-java-profiler.png"
           caption="YourKit Java CPU and Memory Profiler"
           alt="YourKit Java CPU and Memory Profiler"
           width="650" %}

The analysis tool that comes with [YourKit Java Profiler][12] is clear
and intuitive, which makes it easy to get started with the tool and
find out the root cause of your performance issue.

## Eclipse MAT Analyser
The [Eclipse Memory Analyzer][16] is a fast and feature-rich Java heap
analyzer that helps you find memory leaks and reduce memory consumption.

{% include image.html name="eclipse-mat.png"
           caption="Eclipse Memory Analyzer for Java heap analysis"
           alt="Eclipse Memory Analyzer for Java heap analysis"
           width="650" %}

Use the [Eclipse Memory Analyzer][16] to analyze productive heap dumps
with hundreds of millions of objects, quickly calculate the retained
sizes of objects, see who is preventing the Garbage Collector from
collecting objects, run a report to automatically extract leak suspects.

## IBM Thread Analyzer
[IBM Thread and Monitor Dump Analyzer][17] could help you with javacore
and thread activities analyzes in order to identify the root cause of
hangs, deadlocks, and resource contention or monitor bottlenecks.

{% include image.html name="ibm-thread-analyzer.png"
           caption="IBM Thread and Monitor Dump Analyzer for Java"
           alt="IBM Thread and Monitor Dump Analyzer for Java"
           width="650" %}

It analyzes each thread information and provides diagnostic information,
such as:
* current thread information
* the signal that caused the javacore
* Java heap information
* number of runnable threads
* total number of threads
* number of monitors locked, and deadlock information

In addition, [IBM Thread and Monitor Dump Analyzer][17] could help you
with recommended size of the Java heap cluster.

## GCeasy
[GCeasy][9] is a great real-time garbage collection log analyser,
which could help you to figure out whether your application is suffering
from a memory leak or any other GC problems.<br/>

{% include image.html name="gceasy.png"
           caption="GCEasy Reports Potential GC Issues"
           alt="GCEasy Reports Potential GC Issues"
           width="650" %}

[GCeasy][9] applies several intelligence patterns to analyse your GC logs,
and as a result in few seconds you can get a fancy visualized report
with recommendations, key performance indicators and GC statistics
section.

## Apache JMeter
Identifying the current performance bottlenecks you might need to
reproduce them on your local performance stand. For that purpose I use
[Apache JMeter][10] - very simple and powerful [open source][11]
desktop Java application, designed to load test and measure performance.

{% include image.html name="apache-jmeter.png"
           caption="Apache JMeter designed to load test and measure
                                  performance"
           alt="Apache JMeter designed to load test and measure
                                                  performance"
           width="650" %}

It can be used to simulate loads of various scenarios and output
performance data in several ways, including CSV and XML files, and
graphs.

## Splunk
Analyzing a particular issue or keeping an eye on your performance
trends, you often can find yourself parsing megabytes of your
application's logs. Don't be super nerdy inventing a wheel with your
homemade log parser, start using [Splunk][14] - a great solution for
your logs indexing, analysis and visualization.

{% include image.html name="splunk.png"
           caption="Splunk can index, analyze and visualize a statistics
                               from your logs"
           alt="Splunk can index, analyze and visualize a statistics
           from your logs"
           width="650" %}

[Splunk][14] has a free version with [limited][15] functionality, and,
if your logs' volume is not exceeding 500MB per day, you can happily use
this powerful tool for free.

[1]: https://www.jetbrains.com/idea
[2]: https://docs.oracle.com/javase/6/docs/technotes/tools/share/jvisualvm.html
[3]: http://docs.oracle.com/javase/7/docs/technotes/guides/management/agent.html
[4]: http://www.oracle.com/webfolder/technetwork/tutorials/obe/fmw/coherence/Coherence_JVisualVM/JVisualVMPlugin.html
[5]: http://www.eclipse.org
[6]: https://www.jetbrains.com/help/idea/debugging.html
[7]: https://www.jetbrains.com/help/idea/refactoring-source-code.html
[8]: https://www.jetbrains.com/help/idea/auto-completing-code.html
[9]: http://gceasy.io
[10]: http://jmeter.apache.org
[11]: https://github.com/apache/jmeter
[12]: https://www.yourkit.com
[13]: https://visualvm.github.io/plugins.html
[14]: https://www.splunk.com
[15]: https://www.splunk.com/en_us/products/splunk-enterprise/free-vs-enterprise.html
[16]: http://www.eclipse.org/mat
[17]: https://www.ibm.com/developerworks/community/groups/service/html/communitystart?communityUuid=2245aa39-fa5c-4475-b891-14c205f7333c
