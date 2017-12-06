---

copyright:
  years: 2017
lastupdated: "2017-09-13"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 使用 HTTPS 创建和遍历图形

{{site.data.keyword.composeForJanusGraph_full}} 随附一个样本图形数据库“The Graph of the Gods”。本教程使用该样本数据库来说明一些 JanusGraph 概念。请遵循步骤，以使用 CURL 发送 Gremlin 命令来创建、打开和遍历图形。要查看图形的可视化表示形式，请参阅 JanusGraph [入门](http://docs.janusgraph.org/latest/getting-started.html)文档。 

## 1. 连接到 Compose for JanusGraph

要使用 CURL 连接到 {{site.data.keyword.composeForJanusGraph}} 服务，需要使用“连接字符串”。“连接字符串”包含连接到服务所需的用户名、密码、服务器和端口号。您可以在服务仪表板的_概述_页面上找到它。

在开始教程之前，请通过传递简单的 Gremlin 命令来验证是否可以使用“连接字符串”进行连接：

```
curl -XPOST -d '{"gremlin" : "1+1" }' "<CONNECTION STRING>"
```

如果请求成功，那么 JanusGraph 会返回类似于以下内容的响应：

```
{
  requestId":"d2fe6ac7-95d9-4c79-a254-123648cad54d",
  "status":{
    "message":"",
    "code":200,
    "attributes":{}
  },
  "result":{
    "data":[2],
    "meta":{}
  }
}
```

现在，您已准备好开始使用 The Graph of the Gods。

将连接字符串添加到教程中每个 CURL 命令的末尾。
{: .tip}

## 2. 创建图形

{{site.data.keyword.composeForJanusGraph}} 有一个专门的图形工厂，用于创建、打开和关闭图形。使用此工厂 `ConfiguredGraphFactory` 意味着您不需要了解底层存储机制，所以创建新图形只是对其进行命名的问题。我们从创建新图形 _Example_ 开始。

JanusGraph 沙箱需要您声明要使用的所有变量。要声明变量，请使用 `def` 关键字。例如，要声明图形变量：

```
curl -XPOST -d '{"gremlin": "def graph=ConfiguredGraphFactory.create(\"example\");0;"}'
```

{{site.data.keyword.composeForJanusGraph}} 图形名称只能包含字母数字字符和下划线字符。
{: .tip}

请注意，curl 命令以 `;0;` 结尾。这是一个变通方法，因为 HTTP 请求 API 会发出错误：(`{"message":"Cannot get namespace of root","Exception-Class":"java.lang.IllegalArgumentException"}`)，即使它已正确完成操作也是如此。这仅会在代码尝试返回图形时影响图形的类型。

## 3. 打开图形

在您创建图形后，需要打开图形才能使用它。您可以通过在 `JanusGraphConfiguredFactory` 上调用 `open()` 来实现此操作。让我们打开“example”图形。

```
curl -XPOST -d '{"gremlin": "def graph=ConfiguredGraphFactory.open(\"example\");0;"}'
```

##  4. 装入 The Graph of the Gods

要装入样本图形，您需要调用 `GraphOfTheGods.loadWithoutMixedIndex()` 方法，将其传递到已打开的“example”图形，此时会为您创建模型。

```
curl -XPOST -d '{"gremlin": "def graph=ConfiguredGraphFactory.open(\"example\"); GraphOfTheGodsFactory.loadWithoutMixedIndex(graph,true);"}'
```

## 5. 准备遍历图形

现在，您已创建并打开了一个图形，并在其中装入了一些数据，您可以探查图形。要进行移动并查询图形，需要遍历源，通过在图形对象上调用 `traversal()` 可以获取源。这表示任何命令的完整前缀将是：

```
curl -XPOST -d '{"gremlin": "def graph=ConfiguredGraphFactory.open(\"example\");def g=graph.traversal();0;"}'
```

通常，对于图形使用 `graph`，对于遍历源使用 `g`。如果您只需要遍历源，那么可以将命令压缩为：

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal();0;"}'
```

## 6. 遍历图形以查找 Saturn

The Graph of the Gods 图形包含名称属性的全局索引。这种索引通常是进入图形中特定点的第一步。例如，要在图形中查找“saturn”，请执行以下操作：

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next()"}'
```

此命令返回具有属性 `name: saturn` 的顶点。

```
{
  "requestId":"34d349a7-fa8e-4268-8b8d-a8adc0056bf3",
  "status":{
    "message":"",
    "code":200,
    "attributes":{}
  },
  "result":{
    "data":[
      {
        "id":4184,
        "label":"titan",
        "type":"vertex",
        "properties":{
          "name":[
            {
              "id":"16z-388-sl",
              "value":"saturn"
            }
          ],
          "age":[
            {
              "id":"1l7-388-35x",
              "value":10000
            }
          ]
        }
      }
    ],
  "meta":{}
  }
}
```

## 7. 遍历图形以查找 Saturn 的子代

要进一步利用该示例，通过在变量“saturn”中定义“saturn”顶点来使用它，您可以询问哪些顶点使用指向它的“father”标签存储边。使用 `in("father")` 方法将选择这些传入边，并使用 `values("name")` 返回该顶点的名称属性的值。

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); ;g.V(saturn).in(\"father\").values(\"name\")"}'
```

查询将在图形中返回 Saturn 子代的名称，尽管只有一个子代：“jupiter”。

```
{
  "requestId":"2ad6c01e-f724-4e0a-8895-4e8e368fb7ca",
  "status":{
    "message":"",
    "code":200,
    "attributes":{}
  },
  "result":{
    "data":[
      "jupiter"
    ],
    "meta":{}
  }
}
```

如果再次包含 `in("father")` 操作，那么您的遍历会将您带到 Saturn 顶点的孙代。

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); ;g.V(saturn).in(\"father\").in(\"father\").values(\"name\")"}'
```

使用此查询，您会发现 Saturn 的孙代名称是“hercules”。

## 8. 遍历图形 - 位置

在 GraphOfTheGods 中，一些边具有“place”属性，包含纬度和经度。此属性可用于地理定位事件。标签为“battled”的边使用 place 属性来代表发生战争的地点。要查找在雅典（纬度：37.97 和经度：23.72）50 公里以内发生的所有战争，请在图形的边搜索落在这些坐标半径范围内的“place”属性的任何值。

您可以使用 `has("place", geoWithin(Geoshape.circle(37.97, 23.72, 50)))` 来选择这些边。

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); g.E().has(\"place\", geoWithin(Geoshape.circle(37.97, 23.72, 50)))"}'
```

查询返回传入和传出顶点及其标识的边标签。为了使响应立即可读，同时为了找出参与这两场战争的人员，我们可以使用 `as()` 来隐藏作为 _god1_ 和 _god2_ 返回的值，然后使用 `select()` 来检索这些值，使用 `by("name")` 来抽取参与每场战争的两个神的名称。

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); g.E().has(\"place\", geoWithin(Geoshape.circle(37.97, 23.72, 50))).as(\"source\").inV().as(\"god2\").select(\"source\").outV().as(\"god1\").select(\"god1\", \"god2\").by(\"name\")"}'
```

```
"result":{
  "data":[
    {
      "god1":"hercules",
      "god2":"nemean"
    },
    {
      "god1":"hercules",
      "god2":"hydra"
    }
  ],
  "meta":{}
}
```

## 9. 遍历图形 - 顶点

在之前的步骤中，您使用了两个 `in()` 元素来标识 Hercules 是 Saturn 的孙子。该遍历还可以表示为循环，因为 Gremlin 可以 `repeat` 谓词：

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); def hercules=g.V(saturn).repeat(__.in(\"father\")).times(2).next()"}'
```

您可以执行使用 Hercules 顶点作为起点的高级操作。例如，您可以：

- 遍历标签为“father”和“mother”的边，并确定 Hercules 父母的“type”：

  ```
  curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); def hercules=g.V(saturn).repeat(__.in(\"father\")).times(2).next(); g.V(hercules).out(\"father\", \"mother\").label();" }'
  ```

- 遍历“battled”边以查看 Hercules 与谁战斗：

  ```
  curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); def hercules=g.V(saturn).repeat(__.in(\"father\")).times(2).next(); g.V(hercules).out(\"battled\").valueMap()" }' 
  ```

- 遍历“battled”边以查看 Hercules 与谁战斗超过一次：

  ```
  curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); def hercules=g.V(saturn).repeat(__.in(\"father\")).times(2).next(); g.V(hercules).outE(\"battled\").has(\"time\", gt(1)).inV().values(\"name\")" }' 
  ```
  
