---

copyright:
  years: 2019
lastupdated: "2019-07-31"

keywords: janusgraph, compose

subcollection: compose-for-janusgraph

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Metrics
{: #metrics}

Each {{site.data.keyword.composeForJanusGraph_full}} deployment has a _Metrics_ tab which displays information about how each capsule of the deployment is behaving. The graphs are interactive. You can scan through the chart for particular numbers at a point in time, or you can set the view to a range of time periods from the last 30 minutes to the last seven days.

There are graphs for data members and the HAProxy portals on a deployment. Each graph has a name in the upper-left with the specific capsule it describes. 

## Memory Limit

This is the absolute maximum memory that is available to this capsule. By design, we allocate as much memory as possible to any deployment, in excess of its actual provisioned levels. This is a hard limit. Databases may expand to fill that memory.

## Memory Usage

This graph tracks the overall memory usage within the capsules. Note that by design this will tend to be as full as possible; applications, cache, and any other memory usage is included in the memory usage count.

## Swap

Over the lifetime of the capsule, there will be times when data is written to the swap. This indicator shows the amount of available swap space being used by the process. The [swappiness](https://en.wikipedia.org/wiki/Swappiness) value of capsules is set so that it may swap out memory before memory limits have been hit. It makes for improved reliability and better resource use, but it also means that swap usage with capsules is normal. It should only be of concern if the percentage used is very high. 

## Memory Failcnt

The failcnt is the number of requests for more memory that were denied due to memory limits being hit. The value shown is the failcnt per second. This rate can rise and fall as a normal part of the operation of a capsule. It can peak during or just before a rescaling operation. A sustained high failcnt may represent an issue. Where a database deployment is scaled based on the amount of disk storage it has, a sustained high failcnt can mean that it needs more disk allocated to it, which will in turn increase RAM available to the capsules.