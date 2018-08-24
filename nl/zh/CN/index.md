---

Copyright:
  years: 2017,2018
lastupdated: "2018-03-27"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Compose for JanusGraph 入门
{: #getting-started-with-compose-for-janusgraph}

{{site.data.keyword.composeForJanusGraph_full}} 是一个可扩展的图形数据库，针对存储和查询高度互连的数据进行了优化，这些数据建模为百万或几十亿个顶点和边。通过 JanusGraph 的 Apache Tinkerpop(TM) 兼容性，可以简单高效地从这些复杂结构中检索数据，这使您能够执行高效的查询，而这些查询在传统的关系数据库中是很难或不可能执行的。{{site.data.keyword.composeForJanusGraph}} 通过为您管理 JanusGraph，使其性能更佳，同时提供一个简单的可扩展部署系统，以实现高可用性和冗余、自动不停止备份等。
{:shortdesc}

有关 {{site.data.keyword.composeForJanusGraph}} 的概述，请参阅 [JanusGraph 概念](./janusgraph-concepts.html)

## 创建 Compose for JanusGraph 服务实例

[创建 {{site.data.keyword.composeForJanusGraph}} 实例](https://console.{DomainName}/catalog/services/compose-for-janusgraph/)。

当您创建服务实例时，请确保选择服务的名称和凭证名称。保持服务处于未绑定状态；您可以稍后使用供应服务时提供的凭证，将应用程序连接到您的服务。[连接 {{site.data.keyword.cloud}} 应用程序](./connecting-bluemix-app.html)中详细描述了各种凭证值以及如何在应用程序中使用这些凭证值。

供应服务实例时，可以选择*标准*或*企业*套餐。使用*企业*套餐，您可以将实例供应到可用的 {{site.data.keyword.composeEnterprise}} 集群中。{{site.data.keyword.composeEnterprise}} 提供企业合规性所需的安全性和隔离，并使用专用网络来确保已部署的数据库的性能。有关更多详细信息，请参阅 [{{site.data.keyword.composeEnterprise}} 文档](/docs/services/ComposeEnterprise/index.html)。

## 管理 Compose for JanusGraph

您可以从服务仪表板管理服务。您可以在此找到有关 {{site.data.keyword.cloud_notm}} Compose 数据库以及如何连接到该数据库的信息。您还可以：
- 管理备份
- 为服务分配更多资源
- 更改服务密码
- 使用白名单来限制对数据库的访问。 

有关更多信息，请参阅[设置](./dashboard-settings.html)。

{{site.data.keyword.composeForJanusGraph}} 依赖于 Cloud Foundry 角色来管理对服务的访问权。只有具有开发者角色的用户才能查看或使用服务仪表板。有关 Cloud Foundry 角色的更多信息，请参阅 [Cloud Foundry 访问权](https://console.{DomainName}/docs/iam/cfaccess.html#cfaccess)和[管理 Cloud Foundry 访问权](https://console.{DomainName}/docs/iam/mngcf.html#mngcf)页面。
{: .tip}

## 连接到 Compose for JanusGraph

您可以使用随服务一起创建的凭证，或者使用服务仪表板*概述*选项卡中提供的连接字符串和命令行，来连接到服务。

如果要从外部 {{site.data.keyword.cloud_notm}} 连接到服务，可以使用提供的连接字符串或命令行。您可以在[连接到 JanusGraph](./connecting-external.html) 中找到更多信息。

## 连接 {{site.data.keyword.cloud_notm}} 应用程序

要将 {{site.data.keyword.cloud_notm}} 应用程序连接到服务，请使用随服务一起创建的凭证。您可以在[连接 {{site.data.keyword.cloud_notm}} 应用程序](./connecting-bluemix-app.html)中查找有关如何将 {{site.data.keyword.cloud_notm}} 应用程序连接到 {{site.data.keyword.composeForJanusGraph}} 服务的信息。

## 使用 JanusGraph 数据浏览器

通过命令行探查图形数据可能是一项复杂任务。{{site.data.keyword.composeForJanusGraph}} 的数据浏览器将一个简单易用的查询构建器与富查询响应卡组合在一起，以作为交互 JSON 视图和可视化图形显示查询结果。要了解更多信息，请阅读[使用 JanusGraph 数据浏览器](./data-browser.html)
