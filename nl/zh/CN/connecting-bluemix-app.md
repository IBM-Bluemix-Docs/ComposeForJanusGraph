---

copyright:
  years: 2017,2018
lastupdated: "2018-05-08"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 连接 {{site.data.keyword.cloud_notm}} 应用程序

要将 {{site.data.keyword.cloud}} 应用程序连接到服务，请使用供应服务时所创建的服务凭证。

## 可用凭证

字段名称|描述
----------|-----------
`deployment_id`|在 Compose 内创建的服务的内部标识。
`name`|数据库部署名称。
`db_type`|服务所提供的数据库类型；在本例中为 `JanusGraph`。
`uri_cli`|`CURL`命令行，用于说明如何使用基于令牌的认证来发送 gremlin 查询。
`uri_cli_1`|连接到数据库实例的第二个 `CURL` 命令行。
`maps`|未使用
`uri`|连接到服务时使用的 `https` URI，包括服务器的主机名和要连接到的端口号。请参阅[连接外部应用程序](./connecting-external.html)。
`uri_direct_1`|用于连接到服务的第二个 `https` URI。
`misc`|包含 `session`、`websocket` 和 `gremlin_console_yaml` 字段的父字段。
`misc.session`|`https` URI，用户可以使用该 URI 来检索可用于与服务交互的 60 分钟认证令牌。请参阅[连接外部应用程序 - 令牌认证](./connecting-external.html#token-authentication)。
`misc.websocket`|`wss` URI，在使用 Websocket 连接到服务时使用。请参阅[连接外部应用程序 - Websocket](./connecting-external.html#websockets)。
`misc.gremlin_console_yaml`|YAML 信息，用于配置 Gremlin Console 以连接到服务。请参阅[连接外部应用程序 - Gremlin Console](./connecting-external.html#gremlin-console)。
{: caption="表 1. Compose for JanusGraph 凭证" caption-side="top"}

## 使用凭证

应用程序需要连接到服务才能使用服务凭证。以下步骤显示如何使用 Node.js 作为示例来执行此操作。

### 将应用程序连接到服务

如果已有 {{site.data.keyword.cloud_notm}} 应用程序，可以将其从 {{site.data.keyword.cloud_notm}} 帐户连接到服务：

1. 登录到 {{site.data.keyword.cloud_notm}} 帐户
2. 从仪表板中，选择要连接到服务的应用程序。
3. 在应用程序_概述_页面的连接面板中，单击**连接新的**或**连接现有**，以连接到新的或现有的 {{site.data.keyword.cloud_notm}} 服务。

  - 如果单击**连接现有**，那么将显示可用服务的列表。选择要将应用程序连接到的服务，然后单击**连接**。
  - 如果单击**连接新的**，请遵循相应步骤来供应新服务实例。从目录中选择服务以进行供应时，_连接到_字段将自动填写为应用程序的名称。

如果您有一个尚未上传到 {{site.data.keyword.cloud_notm}} 的应用程序，那么可以将该应用程序绑定到服务，然后再将其上传到 {{site.data.keyword.cloud_notm}}： 

1. 登录到 {{site.data.keyword.cloud_notm}} 帐户
2. 下载并安装 [Cloud Foundry CLI](https://github.com/cloudfoundry/cli) 工具
3. 在命令行工具中连接到 {{site.data.keyword.cloud_notm}} 并按照提示进行登录。

  ```
  $ cf api https://api.ng.bluemix.net
  $ cf login
  ```

4. 打开应用程序的 `manifest.yml` 文件。

  - 将 `host` 值更改为唯一的值。您选择的主机将确定应用程序 URL 的子域：`<host>.mybluemix.net`。
  - 更改 `name` 值。您选择的值将是显示在 {{site.data.keyword.cloud_notm}} 仪表板中的应用程序的名称。

5. 更新 `manifest.yml` 中的 `services` 值，以与服务的名称匹配。`manifest.yml` 现在看起来如下所示：

  ```
  ---
  applications:
  - name:    compose-janusgraph-sample-service
    host:    compose-janusgraph-sample-service
    memory:  256M
    services:
      - compose-janusgraph-sample-app
  ```

### 访问和使用凭证

您的应用程序需要解析 Cloud Foundry 环境变量。在 Node.js 中，您可以使用 [cfenv](https://www.npmjs.com/package/cfenv) 软件包来执行此操作。

1. 使用 `--save` 来安装软件包，以更新 `package.json`：

  ```
  npm install --save cfenv
  ```

2. 从 Cloud Foundry 环境变量中抽取 {{site.data.keyword.composeForJanusGraph_full}} 凭证。

  ```javascript
  const cfenv = require('cfenv');
  const appenv = cfenv.getAppEnv();

  // Within the application environment (appenv) there's a services object
  const services = appenv.services;

  // The services object is a map named by service so extract the one for JanusGraph
  let janusgraph_services = services["compose-for-janusgraph"];

  // We now take the first bound JanusGraph service and extract its credentials object
  let credentials = janusgraph_services[0].credentials;
  ```

  现在，您可以使用服务凭证。例如，要获取可用于获取认证令牌的会话 URI：

  ```javascript
  let jgurl = credentials.misc.session[0];
  ```

3. 将应用程序推送到 {{site.data.keyword.cloud_notm}}。推送应用程序时，它将自动绑定到服务。

  ```
  $ cf push
  ```

有关将应用程序与 {{site.data.keyword.cloud_notm}} 服务配合使用的更多信息，请参阅[将服务添加到应用程序](https://console.{DomainName}/docs/services/reqnsi.html#add_service)。
