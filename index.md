---

Copyright:
  Years: 2017
lastupdated: "2017-09-13"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Getting started with Compose for JanusGraph
{: #getting-started-with-compose-for-janusgraph}

{{site.data.keyword.composeForJanusGraph_full}} is a scalable graph database that is optimized for storing and querying highly interconnected data that is modeled as millions or billions of vertices and edges. Simple and efficient retrieval of data from these complex structures is enabled by JanusGraph’s Apache Tinkerpop(TM) compatibility, which allows you to perform efficient queries that would be difficult or impossible with a traditional relational database. {{site.data.keyword.composeForJanusGraph}} makes JanusGraph even better by managing it for you, offering an easy, scalable deployment system that delivers high availability and redundancy, automated no-stop backups and much more.
{:shortdesc}

## Creating a Compose for JanusGraph service instance

[Create a {{site.data.keyword.composeForJanusGraph}} instance](https://console.bluemix.net/catalog/services/compose-for-janusgraph/).

When you create an instance of the service, ensure that you choose both a name for your service and a credential name. Leave the service unbound; you can connect an application to your service later by using the credentials that are provided when the service is provisioned. The various credential values and how to use them in an application are detailed in [Connecting a Bluemix application](./connecting-bluemix-app.html).

When you provision your service instance, you can choose the *Standard* or *Enterprise* plans. With the *Enterprise* plan, you can provision your instance into an available {{site.data.keyword.composeEnterprise}} cluster. {{site.data.keyword.composeEnterprise}} provides the security and isolation required by enterprise compliance and uses dedicated networking to ensure the performance of the deployed databases. See the [Compose Enterprise documentation](../ComposeEnterprise/index.html) for more details.

## Managing Compose for JanusGraph

You can manage your service from the service dashboard. Here you can find information about your Bluemix Compose database and how to connect to it. You can also manage your backups from the dashboard and allocate more resources for your service. For more information, see [Service Overview](./dashboard-overview.html), [Backups](./dashboard-backups.html), and [Scaling Resources](./dashboard-scaling-resources.html).

## Connecting to Compose for JanusGraph

You can connect to your service with the credentials that are created along with the service, or with the connection strings and command line that are provided in the *Overview* tab of your service dashboard.

If you want to connect to your service from outside Bluemix, you can use the provided connection strings or command line. You can find out more in [Connecting to JanusGraph](./connecting-external.html).

## Connecting a Bluemix application

To connect a Bluemix application to your service, use the credentials that are created along with the service. You can find information on how to connect a Bluemix application to a {{site.data.keyword.composeForJanusGraph}} service in [Connecting a Bluemix Application](./connecting-bluemix-app.html).

## Using the JanusGraph Data Browser

Exploring your graph data from the command line can be a complex task. The Data Browser for {{site.data.keyword.composeForJanusGraph}} combines an easy to use Query Builder with rich Query Response Cards that display the results of your queries both as an interactive JSON view and as a visualized graph. To find out more, read [Using the JanusGraph Data Browser](./data-browser.html)