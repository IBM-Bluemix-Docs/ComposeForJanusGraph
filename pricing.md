---

copyright:
  years: 2016,2019
lastupdated: "2019-01-11"

keywords: janusgraph, compose

subcollection: ComposeForJanusGraph

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Pricing
{: #pricing}

## Base Configuration
An {{site.data.keyword.composeForJanusGraph_full}} service starts as a cluster of two JanusGraph Engine nodes, each with 512 MB of memory, which is equal to 2 units of resources. JaunsGraph Storage is a three node cluster where each node has 5 GB of storage, which is equal to 5 units of resources. The service _includes_ replication and high-availablity, so each JanusGraph Engine unit and the price per unit _includes_ the cost of the resources on both the engine nodes. Similarly, each JanusGraph Storage unit and price per unit includes the cost of the resources on the three storage nodes.

The base configuration also includes two HAProxy portals for access, HTTPS, and IP whitelisting. They have 64 MB of memory each.

The base service configuration has a set price. Consult the catalog tiles on {{site.data.keyword.cloud_notm}} for base pricing in your local currency. For example, the base price in US dollars is $116/month. This includes the 5 units for the JanusGraph Storage cluster at $90/month and the 2 units for the JanusGraph Engine at $26/month.

## Increasing resources

The optimal configuration of memory and storage varies across use-cases and workloads. If you need additional storage for large data sets, or additional memory for complex queries, you can increase the storage provided by the JanusGraph Storage cluster and the memory that is provided by the JanusGraph Engine nodes. 

The storage clusters increase in units of 1 GB and the price per unit _includes_ the cost to increase the resources on all the nodes in the cluster. Scaling of storage is available in the _Settings_ tab of the service.
 
The engine nodes increase of units of 256 MB of memory and the price per unit _includes_ the cost to increase the resources on both nodes. Currently, scaling of memory is only available through contacting support.

## Calculating the cost of your deployment
{: #tiered-pricing}

Each additional unit (1 GB) of JanusGraph Storage and each additional unit of JanusGraph Engine memory (256MB) has a per unit price, which is listed in your local currency on the  {{site.data.keyword.cloud_notm}} catalog tile for the service. In US dollars, each unit of JanusGraph Storage is $18 and each unit of JanusGraph Engine is $13. As the _total_ size of all your {{site.data.keyword.composeForJanusGraph}} services increases, the per unit price decreases, as shown in [tiered pricing](#tiered-pricing).

Number of Units of JanusGraph Storage|Price per Unit
----------|-----------
5 - 9 units|Base price per unit - $18.00 USD/Unit of Storage
10 - 24 units|10% reduction - $16.20 USD/Unit of Storage
25 - 49 units|20% reduction - $14.40 USD/Unit of Storage
50 - 99 units|30% reduction - $12.60 USD/Unit of Storage
100 - 499 units|40% reduction - $10.80 USD/Unit of Storage
500 - 999 units|50% reduction - $9.00 USD/Unit of Storage
1,000 - 4,999 units|60% reduction - $7.20 USD/Unit Storage
5,000+ units|70% reduction - $5.40 USD/Unit of Storage
{: caption="Table 1. {{site.data.keyword.composeForJanusGraph}} Storage tiered pricing" caption-side="top"}

## {{site.data.keyword.composeForJanusGraph}} Memory tiered pricing

Number of Units of JanusGraph Engine|Price per Unit
----------|-----------
2 - 9 units|Base price per unit - $13.00 USD/Unit of Engine
10 - 24 units|10% reduction - $11.70 USD/Unit of Engine
25 - 49 units|20% reduction - $10.40 USD/Unit of Engine
50 - 99 units|30% reduction - $9.10 USD/Unit of Engine
100 - 499 units|40% reduction - $7.80 USD/Unit of Engine
500 - 999 units|50% reduction - $6.50 USD/Unit of Engine
1,000 - 4,999 units|60% reduction - $5.20 USD/Unit of Engine
5,000+ units|70% reduction - $3.90 USD/Unit of Engine
{: caption="Table 2. {{site.data.keyword.composeForJanusGraph}} Memory tiered pricing" caption-side="top"}
