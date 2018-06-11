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

# 開始使用 Compose for JanusGraph
{: #getting-started-with-compose-for-janusgraph}

{{site.data.keyword.composeForJanusGraph_full}} 是一個可調式圖形資料庫，已針對儲存及查詢模型化為數百萬或數十億個頂點和邊緣的高度交互連接資料而最佳化。JanuGraph 的 Apache Tinkerpop(TM) 相容性讓您能簡單且有效率地擷取這些複雜結構，如此您可以執行有效率的查詢，而使用傳統關聯式資料庫的話可能會很困難或是不可能達成。{{site.data.keyword.composeForJanusGraph}} 讓 JanusGraph 變得更好，因為它會為您管理、提供容易、可擴充的部署系統，此系統能提供高可用性與備援、自動化備份等等，以及更多其他功能。
{:shortdesc}

如需 {{site.data.keyword.composeForJanusGraph}} 的概觀，請參閱 [JanusGraph 概念](./janusgraph-concepts.html)

## 建立 Compose for JanusGraph 服務實例

[建立 {{site.data.keyword.composeForJanusGraph}} 實例](https://console.bluemix.net/catalog/services/compose-for-janusgraph/)。

當您建立服務的實例時，請確保要選擇服務名稱及認證名稱。請維持服務不連結；稍後使用佈建服務時所提供的認證，即可將應用程式連接至服務。各種認證值以及如何在應用程式中使用它們的詳細資訊，請參閱[連接 {{site.data.keyword.cloud}} 應用程式](./connecting-bluemix-app.html)。

當您佈建服務實例時，可以選擇*標準* 或*企業* 方案。使用*企業* 方案，您可以將您的實例佈建到可用的 {{site.data.keyword.composeEnterprise}} 叢集。{{site.data.keyword.composeEnterprise}} 提供企業相符性所需的安全和隔離，並使用專用網路來確保已部署之資料庫的效能。如需詳細資料，請參閱 [Compose Enterprise 文件](../ComposeEnterprise/index.html)。

## 管理 Compose for JanusGraph

您可以從服務儀表板來管理服務。在這裡，您可以找到 {{site.data.keyword.cloud_notm}} Compose 資料庫的相關資訊，以及連接方式。您也可以：
- 管理備份
- 為您的服務配置更多資源
- 變更服務密碼
- 使用白名單來限制對資料庫的存取權。 

如需相關資訊，請參閱[設定](./dashboard-settings.html)。

{{site.data.keyword.composeForJanusGraph}} 根據 Cloud Foundry 角色來管理對服務的存取。只有具有「開發人員」角色的使用者才能看到或使用服務儀表板。如需 Cloud Foundry 角色的相關資訊，請參閱 [Cloud Foundry 存取](https://console.bluemix.net/docs/iam/cfaccess.html#cfaccess)及[管理 Cloud Foundry 存取](https://console.bluemix.net/docs/iam/mngcf.html#mngcf)頁面。
{: .tip}

## 連接至 Compose for JanusGraph

您可以使用與服務一起建立的認證，或使用服務儀表板的*概觀* 標籤中所提供的連線字串及指令行，來連接至服務。

如果您想要從 {{site.data.keyword.cloud_notm}} 之外連接至服務，則可以使用所提供的連線字串或指令行。您可以在[連接至 JanuGraph](./connecting-external.html) 中進一步瞭解。

## 連接 {{site.data.keyword.cloud_notm}} 應用程式

若要將 {{site.data.keyword.cloud_notm}} 應用程式連接至服務，請使用與服務一起建立的認證。您可以在[連接 {{site.data.keyword.cloud_notm}} 應用程式](./connecting-bluemix-app.html)中，找到如何將 {{site.data.keyword.cloud_notm}} 應用程式連接至 {{site.data.keyword.composeForJanusGraph}} 服務的資訊。

## 使用 JanusGraph 資料瀏覽器

從指令行探索圖形資料可能是複雜的作業。Data Browser for {{site.data.keyword.composeForJanusGraph}} 結合了易於使用的「查詢建置器」，以及將您的查詢結果顯示為互動式 JSON 視圖和視覺化圖形的豐富「查詢回應卡」。若要進一步瞭解，請閱讀[使用 JanusGraph 資料瀏覽器](./data-browser.html)
