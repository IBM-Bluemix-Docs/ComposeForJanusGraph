---

Copyright:
  years: 2017,2019
lastupdated: "2019-01-09"

keywords: janusgraph, compose

subcollection: ComposeForJanusGraph

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# About {{site.data.keyword.composeForJanusGraph}}
{: #about}

{{site.data.keyword.composeForJanusGraph_full}} is a scalable graph database that is optimized for storing and querying highly interconnected data that is modeled as millions or billions of vertices and edges. Simple and efficient retrieval of data from these complex structures is enabled by JanusGraph’s Apache Tinkerpop(TM) compatibility, which allows you to perform efficient queries that would be difficult or impossible with a traditional relational database. {{site.data.keyword.composeForJanusGraph}} makes JanusGraph even better by managing it for you, offering an easy, scalable deployment system that delivers high availability and redundancy, automated no-stop backups and much more.
{:shortdesc}

For an overview of {{site.data.keyword.composeForJanusGraph}}, see [JanusGraph Concepts](/docs/ComposeForJanusGraph?topic=ComposeForJanusGraph-concepts)

## Creating a Compose for JanusGraph service instance

[Create a {{site.data.keyword.composeForJanusGraph}} instance](https://{DomainName}/catalog/compose-for-janusgraph).

When you create an instance of the service, ensure that you choose both a name for your service and a credential name. Then, leave the service unbound. You can connect an application to your service later by using the credentials that are provided when the service is provisioned. The various credential values and how to use them in an application are detailed in [Connecting an {{site.data.keyword.cloud}} application](/docs/ComposeForJanusGraph?topic=ComposeForJanusGraph-ibmcloud-cf-app).

When you provision your service instance, you can choose the *Standard* or *Enterprise* plans. With the *Enterprise* plan, you can provision your instance into an available {{site.data.keyword.composeEnterprise}} cluster. {{site.data.keyword.composeEnterprise}} provides the security and isolation that is required by enterprise compliance and uses dedicated networking to ensure the performance of the deployed databases. See the [{{site.data.keyword.composeEnterprise}} documentation](/docs/ComposeEnterprise?topic=compose-enterprise-about) for more details.

## Managing Compose for JanusGraph

You can manage your service from the service dashboard. Here you can find information about your {{site.data.keyword.cloud_notm}} Compose database and how to connect to it. You can also:
- Manage your backups.
- Allocate more resources for your service.
- Change the service password.
- Use whitelists to restrict access to your databases. 

For more information, see [Settings](/docs/ComposeForJanusGraph?topic=ComposeForJanusGraph-dashboard-settings).

{{site.data.keyword.composeForJanusGraph}} relies on Cloud Foundry roles to manage access to the service. Only users with the Developer role can see or use the service dashboard. For more information about Cloud Foundry roles, see the [Cloud Foundry access](/docs/iam?topic=iam-cfaccess#cfaccess) and the [Managing Cloud Foundry access](/docs/iam?topic=iam-mngcf#mngcf) pages.
{: tip}

## Connecting to Compose for JanusGraph

You can connect to your service with the credentials that are created along with the service, or with the connection strings and command line that are provided in the *Overview* tab of your service dashboard.

If you want to connect to your service from outside {{site.data.keyword.cloud_notm}}, you can use the provided connection strings or command line. You can find out more in [Connecting with HTTP and Websockets](/docs/ComposeForJanusGraph?topic=ComposeForJanusGraph-http-websockets) or [Connecting with the Gremlin Console](/docs/ComposeForJanusGraph?topic=ComposeForJanusGraph-gremlin-console).

## Connecting an {{site.data.keyword.cloud_notm}} application

To connect an {{site.data.keyword.cloud_notm}} application to your service, use the credentials that are created along with the service. You can find information on how to connect an {{site.data.keyword.cloud_notm}} application to a {{site.data.keyword.composeForJanusGraph}} service in [Connecting an {{site.data.keyword.cloud_notm}} Application](/docs/ComposeForJanusGraph?topic=ComposeForJanusGraph-ibmcloud-cf-app).

## Using the JanusGraph Data Browser

Exploring your graph data from the command line can be a complex task. The Data Browser for {{site.data.keyword.composeForJanusGraph}} combines an easy to use Query Builder with rich Query Response Cards that display the results of your queries both as an interactive JSON view and as a visualized graph. To find out more, read [Using the JanusGraph Data Browser](/docs/ComposeForJanusGraph?topic=ComposeForJanusGraph-data-browser)
