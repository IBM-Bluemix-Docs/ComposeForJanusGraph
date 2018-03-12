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

# 連接 {{site.data.keyword.cloud_notm}} 應用程式

若要將 {{site.data.keyword.cloud}} 應用程式連接至您的服務，請使用您在佈建服務時所建立的服務認證。

## 可用的認證

欄位名稱|說明
----------|-----------
`deployment_id`|Compose 內所建立之服務的內部 ID。
`name`|資料庫部署名稱。
`db_type`|服務所提供的資料庫類型；在此情況下，為 `JanusGraph`。
`uri_cli`|說明如何利用記號型鑑別來傳送 gremlin 查詢的 `CURL` 指令行。
`uri_cli_1`|連接至資料庫實例的替代 `CURL` 指令行。
`maps`|未使用
`uri`|連接至服務時所使用的 `https` URI，其中包括伺服器的主機名稱及要連接至的埠號。請參閱[連接外部應用程式](./connecting-external.html)。
`uri_direct_1`|用於連接至服務的替代 `https` URI。
`misc`|包含 `session`、`websocket` 及 `gremlin_console_yaml` 欄位的母項欄位。
`misc.session`| 用來擷取 60 分鐘鑑別記號的 `https` URI，而此鑑別記號可以用來與服務互動。請參閱[連接外部應用程式 - 記號鑑別](./connecting-external.html#token-authentication)。
`misc.websocket`|使用 WebSocket 連接至服務時所使用的 `wss` URI。請參閱[連接外部應用程式 - Websocket](./connecting-external.html#websockets)。
`misc.gremlin_console_yaml`|用來配置 Gremlin 主控台以連接至服務的 YAML 資訊。請參閱[連接外部應用程式 - Gremlin 主控台](./connecting-external.html#gremlin-console)。
{: caption="表 1. Compose for JanusGraph 認證" caption-side="top"}

## 使用認證

您的應用程式必須連接至服務，才能使用服務認證。下列步驟顯示如何使用 Node.js 作為範例來執行此作業。

### 將應用程式連接至服務

如果已有 {{site.data.keyword.cloud_notm}} 應用程式，則可以從 {{site.data.keyword.cloud_notm}} 帳戶將它連接至服務：

1. 登入您的 {{site.data.keyword.cloud_notm}} 帳戶
2. 從儀表板中，選取您要連接至服務的應用程式。
3. 在應用程式_概觀_ 頁面的連線畫面中，按一下**連接新項目**或**連接現有項目**，以連接至新的或現有的 {{site.data.keyword.cloud_notm}} 服務。

  - 如果按一下**連接現有項目**，則會顯示可用服務的清單。選取要連接應用程式的服務，然後按一下**連接**。
  - 如果按一下**連接新項目**，請遵循步驟來佈建新的服務實例。從要佈建的型錄中選取服務時，_連接至_ 欄位將會自動以您應用程式的名稱來完成。

如果您有尚未上傳至 {{site.data.keyword.cloud_notm}} 的應用程式，則可以將應用程式連結至服務，再將它上傳至 {{site.data.keyword.cloud_notm}}： 

1. 登入您的 {{site.data.keyword.cloud_notm}} 帳戶
2. 下載並安裝 [Cloud Foundry CLI](https://github.com/cloudfoundry/cli) 工具
3. 在指令行工具中連接至 {{site.data.keyword.cloud_notm}}，並遵循提示以登入。

  ```
  $ cf api https://api.ng.bluemix.net
  $ cf login
  ```

4. 開啟應用程式的 `manifest.yml` 檔案。

  - 將 `host` 值變更為唯一的值。您選擇的主機將決定應用程式 URL 的子網域：`<host>.mybluemix.net`。
  - 變更 `name` 值。您選擇的值將是應用程式出現在 {{site.data.keyword.cloud_notm}} 儀表板時的名稱。

5. 更新 `manifest.yml` 中的 `services` 值，以符合服務的名稱。`manifest.yml` 現在看起來如下：

  ```
  ---
  applications:
  - name:    compose-janusgraph-sample-service
    host:    compose-janusgraph-sample-service
    memory:  256M
    services:
      - compose-janusgraph-sample-app
  ```

### 存取及使用認證

您的應用程式需要剖析 Cloud Foundry 環境變數。在 Node.js 中，您可以使用 [cfenv](https://www.npmjs.com/package/cfenv) 套件來執行這個動作。

1. 使用 `--save` 來安裝套件，以更新 `package.json`：

  ```
  npm install --save cfenv
  ```

2. 從 Cloud Foundry 環境變數中擷取 {{site.data.keyword.composeForJanusGraph_full}} 認證。

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

  您現在可以使用服務認證。例如，若要取得您可以用來取得鑑別記號的階段作業 URI，請執行下列指令：

  ```javascript
  let jgurl = credentials.misc.session[0];
  ```

3. 將應用程式推送至 {{site.data.keyword.cloud_notm}}。當您推送應用程式時，它會自動連結至服務。

  ```
  $ cf push
  ```

如需使用應用程式與 {{site.data.keyword.cloud_notm}} 服務搭配的相關資訊，請參閱[將服務新增至您的應用程式](https://console.bluemix.net/docs/services/reqnsi.html#add_service)。
