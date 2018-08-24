---

Copyright:
  years: 2017,2018
lastupdated: "2018-05-07"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 服務概觀

_概觀_ 頁面顯示 {{site.data.keyword.cloud}} Compose 資料庫的相關資訊。概觀包括基本識別資訊及現行資源用量。您也會找到一個連線字串區段，您可以與工具搭配使用，或利用工具來連接至您的資料庫。

## 部署詳細資料

_部署詳細資料_ 畫面顯示服務的詳細資料。

![部署詳細資料](./images/janusgraph-deployment-details.png "「部署詳細資料」畫面的視圖")

### 類型

服務所提供的資料庫類型，以及服務使用的資料庫版本。如果有更新的資料庫版本可用，則會顯示通知，並附有一個鏈結，可讓您前往服務儀表板的[升級版本](/docs/services/ComposeForJanusGraph/dashboard-settings.html#upgrade-version)區段。

### ID

服務的內部 ID。

### 用量

服務方案所提供的資料庫大小及儲存空間數量。

## 現行工作

對您的服務進行管理變更（例如調整或建立手動備份）會啟動工作。工作在執行時，_現行工作_ 畫面會出現在_概觀_ 頁面上，同時顯示工作名稱及進度列。工作完成之後，_現行工作_ 畫面便不再出現在_概觀_ 頁面上。

## 連線字串

「連線字串」可以由某些用戶端程式庫使用，且包含其他程式庫進行連接時所需的一切資訊。您可以在[連接外部應用程式](./connecting-external.html)中，瞭解如何使用連線字串來進行連接至您的服務。

您將在_連線字串_ 畫面的另一個標籤中，找到服務的每一個「連線字串」。

### 階段作業

您可以使用階段作業 URI 來取得有效時間為 60 分鐘的授權記號。當使用 HTTPS 或 Websocket URI 來呼叫部署時，您應該使用所提供的記號。

### HTTPS

這是 JanusGraph 部署的基礎連線字串。若要使用 HTTPS 連線字串，您需要提供管理使用者認證來連接至伺服器。如需如何使用 HTTPS 的詳細資料，請參閱[使用 HTTPS 建立及遍訪圖形](./tutorial-https.html)。

### Websocket

Websocket URI 可以用來建立搭配 JanusGraph 部署的長時間執行階段作業。它們的字首為 `wss:`，表示連線會以 HTTPS 保護。這些連線字串需要基本鑑別與管理使用者認證搭配使用，才能連接至伺服器。

有一些 JanusGraph 程式庫會使用 WebSocket 作為其與伺服器的連線；若要使用 {{site.data.keyword.composeForJanusGraph}}，它們必須能夠執行基本鑑別，並使用 WSS（透過 TLS 的安全 WebSocket）。

### Gremlin 主控台 YAML

您可以使用所提供的任一配置，利用「Gremlin 主控台」連接至 {{site.data.keyword.composeForJanusGraph}}。如需如何使用「Gremlin 主控台 YAML」的詳細資料，請參閱[使用 Gremlin 主控台建立及遍訪圖形](./tutorial-gremlin-console.html)。


## 實例管理 API

您可以透過 {{site.data.keyword.cloud_notm}} Compose API 管理 {{site.data.keyword.composeForJanusGraph}} 服務。

### 基礎端點

基礎端點是由服務所在地區及服務實例 ID 所組成。它將位於每個端點的開頭處。

### 部署 ID

大部分呼叫都需要部署 ID，部署 ID 可識別特定的部署實例。

### 參考資料

如需如何在所有 {{site.data.keyword.cloud_notm}} Compose 服務中使用 {{site.data.keyword.cloud_notm}} Compose API 的其他文件及參考資料，請閱讀 [{{site.data.keyword.cloud_notm}} Compose API](https://www.compose.com/articles/the-ibm-cloud-compose-api/)。
