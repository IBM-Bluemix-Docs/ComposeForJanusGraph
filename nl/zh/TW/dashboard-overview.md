---

Copyright:
  Years: 2017
lastupdated: "2017-09-05"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 服務概觀

_概觀_ 頁面顯示 {{site.data.keyword.cloud}} Compose 資料庫的相關資訊。概觀包括基本識別資訊及現行資源使用。您也會找到一個區段，以尋找可與工具搭配使用的連線字串，或利用工具來連接至您的資料庫。

## 部署詳細資料

_部署詳細資料_ 畫面顯示服務的詳細資料。

![部署詳細資料](./images/janusgraph-deployment-details.png "「部署詳細資料」畫面的視圖")

### 類型

服務所提供的資料庫類型，以及服務使用的資料庫版本。

### 名稱

服務的內部 ID。

### 用量

服務方案所提供的資料庫大小及儲存空間數量。


## 連線字串

「連線字串」可以由某些用戶端程式庫使用，且包含其他程式庫進行連接時所需的一切資訊。您可以在[連接外部應用程式](./connecting-external.html)中，瞭解如何使用「連線字串」來連接至您的服務。

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
