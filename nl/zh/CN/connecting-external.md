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

# 连接到 JanusGraph

JanusGraph 支持使用 HTTP 请求或通过 WebSockets 进行连接。所有连接都通过 HTTPS 执行且需要认证。基于 HTTP 的请求支持基本和令牌认证。对于 JanusGraph 部署的一次性调用以外的任何内容，使用 WebSocket 或令牌认证将提高性能。基本认证是一个成本很高的进程，会减慢请求的速度。

有关如何使用 HTTPS 的更多信息以及了解创建和打开图形的更多信息，请参阅[使用 HTTPS 创建和遍历图形](./tutorial-https.html)。
{: .tip}

客户机与数据库之间的实际通信涉及以 Gremlin（Groovy 语言的一种方言，定制用来查询图形结构）发送请求的客户机和对这些查询作出响应的数据库。您还可以使用 Gremlin Console 发送请求。

有关如何获取、安装和使用 Gremlin Console 的详细信息，请参阅[使用 Gremlin Console 创建和遍历图形](./tutorial-gremlin-console.html)。
{: .tip}

## HTTP 请求

如果您正在使用 HTTP 请求来连接到服务器，那么可以使用单个端点与 JanusGraph 进行通信，并设置为在部署中的所有可用节点之间循环。它包括主机名和端口。

您可以从服务仪表板概述页面中获取 HTTPS 连接字符串。连接字符串需要使用管理员用户凭证进行基本认证，以连接到服务器。

要使用 HTTP 请求连接到 {{site.data.keyword.composeForJanusGraph}}，您需要将 HTTP POST 方法与端点配合使用，然后发送 JSON 格式的文档，其中包含键为“gremlin”、值为您希望发送的 Gremlin 查询的单个条目。 

```json
{
    "gremlin": "a gremlin query would go here"
}
```

Gremlin 是一种可扩展遍历语言，而其中的程序也可以简单到“1+1”。要在 `curl` 命令中作为脚本传递，POST 请求的数据部分为：

```
'{"gremlin" : "1+1" }'
``` 

如果您多次将同一脚本发送到服务器，那么可以通过提供带有可用于 gremlin 脚本的变量映射的可选“绑定”字典，来提高性能。这有助于提高性能，因为服务器不必重新编译 gremlin 脚本。
{: .tip}

在 Apache Tinkerpop 文档的[通过 REST 进行连接](http://tinkerpop.apache.org/docs/3.2.3/reference/#_connecting_via_rest)部分中可以找到有关 HTTP 协议的其他信息

要将 Gremlin 脚本发送到端点，您需要进行认证。有两种方式可以通过 HTTP 请求访问进行认证：基本认证和令牌认证。

## 基本认证

基本认证通过请求发送您的用户名和密码。对于一次性查询是合理的，但是不仅如此，您开始加载服务器时需要进行密码交换和验证，这就增加了复杂性。通过 curl，您可以使用 `-u` 选项传递用户名和密码：

```shell
$ curl -XPOST -d '{"gremlin" : "1+1" }' "https://portal91-0.ianuspater.compose-3.composedb.com:17916" -u admin:PASSWORDHERE
```

您还可以在端点 URL 中嵌入用户名和密码。 

## 令牌认证

对于更有效的 HTTP 请求，您可以从另一个端点获取会话令牌。在所有后续请求的头中传递此令牌。如果要在 HTTP 端点上执行多个调用，那么可以通过使用令牌认证来提高性能。

使用服务仪表板概述页面的_会话_面板中的任一连接字符串。连接字符串已包含将令牌作为 curl 命令获取时所需的调用：

```shell
$ curl -XGET "https://admin:PASSWORDHERE@portal91-0.ianuspater.compose-3.composedb.com:17916/session"
```

返回的 JSON 结果包含会话令牌：

```
{
  "token": "YWRtaW46MTQ5NjkyODM1NDgzMzpWYXlxMERsMWs0VmltdGo3WUpzMEwtN3prUi00TGxZR3J6LXZnbDVmN3lnPQ=="
}
```

该令牌的有效期为 60 分钟
{: .tip}

抽取令牌值，然后将其作为 `Authorization` 头包含在任何后续请求中：

```shell
$ curl -XPOST -H "Authorization: Token YWRtaW46MTQ5NjkyODM1NDgzMzpWYXlxMERsMWs0VmltdGo3WUpzMEwtN3prUi00TGxZR3J6LXZnbDVmN3lnPQ==" -d '{"gremlin" : "1+1" }' "https://portal91-0.ianuspater.compose-3.composedb.com:17916"
```

如果要从 shell 执行此操作，并且您有可用的 [`jq`](https://stedolan.github.io/jq/) 命令，那么可以通过运行类似于以下内容的命令来设置环境变量：

```shell
$ export JGTOKEN=`curl -s -XGET "https://admin:PASSWORDHERE@portal91-0.ianuspater.compose-3.composedb.com:17916/session" | jq -r .token`
```

然后，按如下所示运行请求：

```shell
$ curl -XPOST -H "Authorization: Token $JGTOKEN" -d '{"gremlin" : "1+1" }' "https://portal91-0.ianuspater.compose-3.composedb.com:17916"
```

## HTTP JSON 响应

以下查询的响应采用 JSON 文档格式，带有请求标识值、状态对象和结果对象。以下是响应的示例：

```json
{
  "requestId": "1bf4d1d4-eba4-484f-a94b-3cfa85df3448",
  "status": {
    "message": "",
    "code": 200,
    "attributes": {}
  },
  "result": {
    "data": [
      {
        "god1": "hercules",
        "god2": "nemean"
      },
      {
        "god1": "hercules",
        "god2": "hydra"
      }
    ],
    "meta": {}
  }
}
```
`requestId` 是请求的唯一标识。`status` 包含已报告状态的可读消息、HTTP 状态样式代码和其他属性。`result` 包含根据所执行的 Gremlin 代码返回的任何数据构造的数据对象，以及在结果数据范围外的任何额外信息的元部分。

## Websocket

WebSocket 是一种高效的方法，它通过 TCP 和 TLS 创建长期连接的双向连接，这对于客户机应用程序和 JanusGraph 服务器之间的交互式会话而言非常理想。JanusGraph 的许多库使用 WebSocket 作为其与服务器的连接；要使用 JanusGraph on Compose，它们必须能够执行基本认证和使用 WSS（安全 WebSocket over TLS）。 

您可以从服务仪表板概述页面的 _Websocket_部分获取 Websocket 连接字符串。URI 可用来与 JanusGraph 部署建立长时间运行的会话，并以 `wss:` 作为前缀，表示连接为安全的 HTTPS。这些连接字符串需要将基本认证与管理员用户凭证配合使用，以连接到服务器。

## Gremlin Console

Gremlin Console 是使用启用 Tinkerpop 的服务器的重要工具。Gremlin Console 使用 WebSocket 连接，但不使用 URI 语法进行连接；部分原因是它还需要配置其他元素才能使用连接。

要配置 Gremlin Console，请提供具有相应详细信息的 YAML 文件。您可以在服务仪表板概述页面上的 _Gremlin Console YAML_ 面板中找到此配置信息。

有关如何获取、安装和使用 Gremlin Console 的详细信息，请参阅[使用 Gremlin Console 创建和遍历图形](./tutorial-gremlin-console.html)。
