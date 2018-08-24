---

copyright:
  years: 2017,2018
lastupdated: "2017-12-12"
---

# JanusGraph 概念

JanusGraph 是图形数据库。您可以在数据库中创建、查询和修改许多图形。JanusGraph 基于 Apache Tinerpop 堆栈构建，并使用 Gremlin 语言进行遍历、命令和查询。

要阅读有关 JanusGraph 的更多信息，请参阅 [JanusGraph 文档](http://docs.janusgraph.org/latest/index.html)。

<iframe title="Compose for JanusGraph 概述" class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="//www.youtube.com/embed/zTaoMWv6lnE?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

您可以在 [IBM Compose for JanusGraph 学习中心](http://ibm.biz/janusgraph-learning)找到有关 {{site.data.keyword.composeForJanusGraph}} 的更多视频。
{: .tip}

## 图形数据库简介

在最简单的情况下，图形由一组顶点和它们之间的边组成。顶点是图形的基本单位，它表示一些不可分割的对象。实际上，这可能是人员、位置或对象。它可以具有用于描述对象的属性。 

边是两个顶点之间的连接，表示它们之间的关系。它可以具有多重性、方向和属性。

在非图形数据库中，实体之间的关系是数据库的辅助功能，通过字段中的共享键表示或通过 JOIN 创建。这意味着以下关系可能需要多个查询或应用程序执行从更多常规查询组合关系的工作。

在图形数据库中，关系是数据模型的主要组件，并且从顶点到边再到顶点乃至其他边的遍历是用于查询模型中数据的主要机制。这使得图形数据库更适用于涉及类似以下关系的查询：“所有喜欢乐队 X 且有朋友或朋友的朋友购买品牌 Y 的人”。 

在 JanusGraph 中，每个图形都具有唯一名称。要创建、打开、修改和查询图形，请使用 Gremlin 语言。Gremlin 查询由一些命令组成，这些命令可以通过某种方式处理或遍历图形，以提供所需的结果。

## 顶点

顶点是表示对象的图形元素。在创建顶点时，它具有由图形引擎设置的标识以及用户定义的标签。顶点还可以存储建立索引和查询所依据的属性。


有关如何创建、查询顶点的更多信息以及顶点的其他实现详细信息，请参阅 [Tinkerpop/Gremlin 文档](http://tinkerpop.apache.org/docs/3.2.3/reference/#_the_graph_structure)。

## 边

边是表示关系的图形元素。要创建边，用户需要提供传入/传出顶点和标签。图形引擎将为边分配一个标识。边还可以存储建立索引和查询所依据的属性。


有关如何创建、查询边的更多信息以及边的其他实现详细信息，请参阅 [Tinkerpop/Gremlin 文档](http://tinkerpop.apache.org/docs/3.2.3/reference/#_the_graph_structure)。

## 属性

属性是一个键/值对，用于描述、命名边或顶点，或者与边或顶点相关联。边和顶点可以有多个属性。例如，如果顶点的类型为“human”，那么可以向该顶点分配“name”和“age”属性。

可以对属性建立索引，以便顶点和边可由其属性检索，例如获取所有同名的顶点。

边的属性可能类似于“weight”，因此通过多个不同权重的边对图形顶点进行遍历可以计算出行程的总权重。 

## 遍历

遍历是分析图形结构的过程。遍历过程会发现并返回有关边、顶点及其属性的信息。要了解不同的遍历步骤和算法，请参阅[有关遍历的 Tinkerpop/Gremlin 文档](http://tinkerpop.apache.org/docs/3.2.3/reference/#traversal)。

## Gremlin

Gremlin 分为两个部分：语言和服务器。

Gremlin 语言是用于交互和查询图形的图形遍历语言。

Gremlin 服务器是处理本地或远程 Gremlin 语言查询的服务器的规范。实现它是 Apache Tinkerpop 的一部分。

要执行 Gremlin 语言查询，请将 Gremlin 查询发送到相应的 Gremlin 服务器。在 Compose 上，那是作为 JanusGraph 部署一部分运行的服务器。

## Apache TinkerPop

Apache Tinkerpop 是一种开放式源代码图形计算框架。TinkerPop 是将数据作为图形进行建模并与 Gremlin 语言命令进行交互以创建、遍历和处理图形的系统。要了解有关这些部分如何组合在一起的更多信息，请参阅有关[图形系统集成](http://tinkerpop.apache.org/docs/3.2.3/reference/#_graph_system_integration)的 TinkerPop/Gremlin 文档。
