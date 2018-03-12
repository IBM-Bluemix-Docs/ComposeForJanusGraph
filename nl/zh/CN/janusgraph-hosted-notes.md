---
copyright:
  years: '2015, 2017'
lastupdated: '2017-11-21'
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# JanusGraph 托管服务说明

{{site.data.keyword.composeForJanusGraph_full}} 是一种托管服务，因此在一些情况下，确保部署安全性与在专用服务器上运行的 JanusGraph 不同。本主题重点说明这些差异和变化。其中大部分更改会影响 Gremlin 语言查询的工作方式。

## 沙箱

为了确保 Gremlin 查询不会访问可能危害系统的功能，所有查询均在沙箱中运行。这意味着所有函数和方法调用都必须具有与以下其中一个正则表达式匹配的签名：

```
java\.util.*
java\.lang\.Object.*
java\.lang\.Class <T extends java\.lang\.Object>#getSimpleName\(
java\.lang\.Iterable <T extends java\.lang\.Object>#iterator\(\)
java\.lang\.Boolean(?!#getBoolean\().*
java\.lang\.Double.*
java\.lang\.Float.*
java\.lang\.Integer(?!#getInteger\().*
java\.lang\.Long(?!#getLong\().*
java\.lang\.Math.*
java\.lang\.String(?!#intern\(\)).*
java\.lang\.StringBuilder.*
java\.lang\.Object#equals\(
java\.lang\.Object#hashCode\(
java\.util\.ArrayList#equals\(

org\.codehaus\.groovy\.runtime\.DefaultGroovyMethods.*
org\.codehaus\.groovy\.runtime\.StringGroovyMethods.*
org\.apache\.commons\.configuration\.MapConfiguration.*
org\.apache\.tinkerpop\.gremlin\.structure(?!\.io|\.Graph#toString\(|\.Graph#variables\(|\.Graph#configuration\(|\.Graph#io\(\)|\.util\.GraphFactory.*).*
org\.apache\.tinkerpop\.gremlin\.process\.traversal.*
org\.apache\.tinkerpop\.gremlin\.util\.Gremlin#version\(\)
org\.janusgraph\.core(?!\.JanusGraphFactory.*).*
org\.janusgraph\.graphdb(?!\.tinkerpop\.JanusGraphBlueprintsGraph#toString\(|\.tinkerpop\.JanusGraphBlueprintsGraph#variables\(|\.tinkerpop\.JanusGraphBlueprintsGraph#configuration\(|\.tinkerpop\.JanusGraphBlueprintsGraph#io\().*
org\.janusgraph\.example\.GraphOfTheGodsFactory(?!#create\(|#main\().*
org\.apache\.tinkerpop\.gremlin\.groovy\.plugin\.dsl\.credential\.CredentialGraph.*

Script\d+#.+\(?.+\)
groovy\.lang\.Closure <V extends java\.lang\.Object>#call\(?.+\)

org\.janusgraph\.util\.stats\.MetricManager#getRegistry\(\)
org\.janusgraph\.util\.stats\.MetricManager#INSTANCE
org\.apache\.tinkerpop\.gremlin\.util\.MetricManager#getRegistry\(\)
org\.apache\.tinkerpop\.gremlin\.util\.MetricManager#INSTANCE
```

尝试运行与函数白名单中的任何条目都不匹配的函数或方法调用会导致错误： 

```
[Static type checking] - Not authorized to call this method: ...
```

沙箱还要求对所有变量进行静态声明以启用安全检查。您可以使用 `def` 命令来执行此操作。

## 会话

会话支持连接到 {{site.data.keyword.composeForJanusGraph}} 以维护请求之间的状态。{{site.data.keyword.composeForJanusGraph}} 的大多数连接选项都没有关联的会话。这意味着通过这些连接方法发送的任何 Gremlin 脚本都需要完全自包含。例如，打开脚本需要使用的任何图形、查询相应的节点以及从这些节点遍历图形，这些操作全部需要在一个请求中进行处理。此限制适用于 HTTP 请求和 WebSocket 连接。 

对此的例外情况是，使用 `:remote connect` 通过 Gremlin 控制台建立连接。

```
:remote connect tinkerpop.server conf/compose.yaml
```

如果使用上述命令建立连接，那么不会有任何会话，并且连接的操作方式与 HTTP 请求相同。但是，如果将 `session` 添加为自变量，那么现在会启用会话支持。

```
:remote connect tinkerpop.server conf/compose.yaml session
```

使用会话可以在一个命令中定义一个变量，然后在其他命令中引用该变量。
{: .tip}

## 事务

对底层 JanusGraph 数据库的所有更改都封装在一个事务中。事务支持落实其内部的任何更改以使更改持久，或者支持回滚更改以确保不更改。 

通过 HTTP 请求或通过 WebSocket 发起请求时，如果进行的更改将写入数据库，那么会自动启动事务。请求完成后，该事务还会自动落实。

例外情况是使用启用了会话的 Gremlin 控制台进行连接。会话可以长期存在，因此由用户决定通过调用 `graph.tx().commit()` 来落实任何更改；其中，`graph` 是包含更改的已打开图形。这也意味着可以调用 `graph.tx().rollback()` 来返回到事务启动之前的图形状态。 

## 混合索引

当前版本的 {{site.data.keyword.composeForJanusGraph}} 不支持混合索引。
