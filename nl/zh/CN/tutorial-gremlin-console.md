---

copyright:
  years: 2017,2018
lastupdated: "2017-09-13"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 使用 Gremlin Console 创建和遍历图形

本主题将帮助您开始使用第一个 {{site.data.keyword.composeForJanusGraph_full}} 数据库。对于本教程，我们将使用 Gremlin Console 向 JanusGraph 服务器发送命令。

## 1. 安装 Gremlin

要安装 Gremlin Console，请执行以下操作：

1. 请确保您已安装最新版本的 Java。
2. 下载 [Gremlin Console V3.2.3](https://archive.apache.org/dist/tinkerpop/3.2.3/apache-tinkerpop-gremlin-console-3.2.3-bin.zip)。
3. 将下载的文件解压缩到工作目录。
4. 使用命令行或终端，将目录更改为根 Gremlin Console 目录，并通过执行 `bin/gremlin.sh` 来验证下载：

  ```text
  $ unzip -q  ~/Downloads/apache-tinkerpop-gremlin-console-3.2.3-bin.zip
  $ cd apache-tinkerpop-gremlin-console-3.2.3
  $ bin/gremlin.sh

          \,,,/
          (o o)
  -----oOOo-(3)-oOOo-----
  plugin activated: tinkerpop.server
  plugin activated: tinkerpop.utilities
  plugin activated: tinkerpop.tinkergraph
  gremlin> [CONTROL-D]                                                             $

  ```

5. 配置 Gremlin Console。您可以在 {{site.data.keyword.composeForJanusGraph}} 服务的*概述*页面上找到您的 Gremlin Console YAML。将其中一个配置保存到 `conf` 目录中的某个文件。在此示例中，将保存为 `conf/compose.yaml`。
 
## 2. 连接到 Compose for JanusGraph

要验证连接，请再次执行 `bin/gremlin.sh`。然后，使用 `:remote` 命令连接到 tinkerpop 服务器：

```text
:remote connect tinkerpop.server conf/compose.yaml session
```

此命令告诉 Gremlin 使用 `conf/compose.yaml` 中的配置，`connect` 到作为 `tinerpop.server` 的服务器。我们还通过另一个自变量 - `session` - 启用会话支持。

输入 `:help :remote` 以查看 `:remote` 命令的选项。
{: .tip}

如果连接成功，您将看到 SSL 警告和消息以确认连接。

```text
WARN  org.apache.tinkerpop.gremlin.driver.Cluster  - SSL configured without a trustCertChainFile and thus trusts all certificates without verification (not suitable for production)
==>Configured portal93-4.ianuspater.compose-3.composedb.com/159.8.153.216:18045-[2378d0de-0a93-4330-b8d8-848667f5b117]
gremlin>
```

## 3. 向服务器发送命令。

Gremlin Console 不会自动将所有内容转发到远程服务器：要将命令发送到服务器，您需要在命令前加上 `:>` 前缀。或者，可以使用 `:remote console` 将所有控制台命令重定向到远程服务器。通过以下命令和响应序列，可演示此操作：

```text
$ bin/gremlin.sh                                                                   

        \,,,/
        (o o)
-----oOOo-(3)-oOOo-----
plugin activated: tinkerpop.server
plugin activated: tinkerpop.utilities
plugin activated: tinkerpop.tinkergraph
gremlin> 1+1
==>2
gremlin> :> 1+1
==>No remotes are configured.  Use :remote command.
gremlin> :remote connect tinkerpop.server conf/compose.yaml session
WARN  org.apache.tinkerpop.gremlin.driver.Cluster  - SSL configured without a trustCertChainFile and thus trusts all certificates without verification (not suitable for production)
==>Configured portal93-4.ianuspater.compose-3.composedb.com/159.8.153.216:18045-[016d7a68-cf70-450e-92eb-4e5e2a647b5b]
gremlin> :remote console
==>All scripts will now be sent to Gremlin Server - [portal93-4.ianuspater.compose-3.composedb.com/159.8.153.216:18045]-[016d7a68-cf70-450e-92eb-4e5e2a647b5b] - type ':remote console' to return to local mode
gremlin> 1+1
==>2
gremlin> 

```

## 4. 创建图形

{{site.data.keyword.composeForJanusGraph}} 有一个专门的图形工厂，用于创建、打开和关闭图形。使用此工厂 `ConfiguredGraphFactory` 意味着您不需要了解底层存储机制，所以创建新图形只是对其进行命名的问题。我们从创建新图形开始。在本教程中的样本命令中，我们将调用图形 _mygraph_。

使用 `def` 关键字来声明图形变量。

```
def graph=ConfiguredGraphFactory.create("mygraph")
```

{{site.data.keyword.composeForJanusGraph}} 图形名称只能包含字母数字字符和下划线字符。
{: .tip}

## 5. 打开图形

在您创建图形后，需要打开图形才能使用它。您可以通过在 `ConfiguredGraphFactory` 上调用 `open()` 来实现此操作。让我们打开图形：

```
def graph=ConfiguredGraphFactory.open("mygraph")
```

此时，我们需要落实该操作，以便图形持续存在：

```
graph.tx().commit()
```

如果我们现在就结束会话，那么当我们回来时，新图形将会出现。

## 6. 将顶点添加到图形

我们的图形将包含有关软件开发者和他们开发的软件的信息。让我们添加一个名为“marko”的人员和一个名为“lop”的软件。

首先，我们来添加 marko。

```
def v1 = graph.addVertex(T.label, "person", "name", "marko", "age", 29)
```

如果成功，此命令将返回我们添加的顶点的索引 - `==>v[4304]` - 我们可以继续添加第二个顶点。

```
def v2 = graph.addVertex(T.label, "software", "name", "lop", "lang", "java")
```

再次，我们需要落实这些操作。

```
graph.tx().commit()
```

## 7. 在图形中查找顶点

要查找顶点（如名称为“marko”的顶点），首先遍历图形，然后使用 has() 方法，并返回顶点的标识。

```
def g=graph.traversal()
g.V().has("name","marko")
```

要将顶点重新分配给变量 v1 和 v2，例如，如果我们已关闭旧会话并启动了新会话，那么我们可使用 next()。

```
def v1 = g.V().has("name","marko").next()
def v2 = g.V().has("name","lop").next()
```

然后，在后续的 Gremlin 命令中可以使用这些变量。

## 8. 添加边

接下来，我们可以在两个顶点之间添加一个加权边，为边提供标签“created”和权重 0.4。

请注意，您需要通过将值指定为 `0.4d` 来将数字强制转换为小数。

```
v1.addEdge("created", v2, "weight", 0.4d)
```

同样，确保落实更改。

## 9. 查询图形

现在，您可以查询图形以查找 Marko 所创建的任何软件的名称。

```
g.V().has("name","marko").out("created").values("name")
```
