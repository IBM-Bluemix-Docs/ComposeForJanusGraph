---

copyright:
  years: 2017,2018
lastupdated: "2017-10-16"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 備份
{: #backups}

您可以從服務儀表板的_管理_ 頁面的_備份_ 標籤中，建立及下載備份。有每日、每週、每月及隨需應變備份可供使用。系統會根據下列排程保留它們：

備份類型|保留排程
----------|-----------
每日|每日備份保留 7 日
每週|每週備份保留 4 週
每月|每月備份保留 3 個月
隨需應變|保留一份隨需應變備份。保留的備份一律是最新的隨需應變備份。
{: caption="表 1. 備份保留排程" caption-side="top"}

備份排程與保留原則是固定的。如果您需要保留的備份數目超過保留排程所容許的數目，則應該根據您的商業需求下載備份及保留保存檔。

## 檢視現有備份

資料庫的每日備份是自動排定的。若要檢視現有備份，請執行下列動作：

1. 導覽至服務儀表板的_管理_ 頁面。
2. 按一下標籤中的**備份**，以開啟_備份_ 頁面。即會顯示可用備份的清單，最新備份位於清單頂端：

  ![可用備份](./images/janusgraph-backups-show.png "可用備份的清單，包括擱置備份")

按一下對應列來展開任何可用備份的選項。
  ![備份選項](./images/janusgraph-backups-options.png "備份的選項。") 

### 使用 API 檢視現有備份

備份的清單位於 `GET /2016-07/deployments/:id/backups` 端點上。具有服務實例 ID 及部署 ID 的「基礎端點」都會顯示在服務的_概觀_ 中。例如： 
``` 
https://composebroker-dashboard-public.mybluemix.net/api/2016-07/instances/$INSTANCE_ID/deployments/$DEPLOYMENT_ID/backups
```  

## 建立手動備份

除了排定的備份外，您也可以手動建立備份。若要建立手動備份，請遵循步驟來檢視現有備份，然後在可用備份的清單上方按一下**立即備份**。即會顯示一則訊息，讓您知道已起始備份，且已將「擱置」備份新增至可用備份的清單。

### 使用 API 建立備份

請將 POST 要求傳送至 backups 端點，以起始手動備份：`POST /2016-07/deployments/:id/backups`。它會立即傳回執行中備份的秘訣 ID 及相關資訊。您必須檢查 backups 端點，以查看備份是否已完成，並在使用之前找到其 backup_id。請使用 `GET /2016-07/deployments/:id/backups/`。

## 還原備份

若要將備份還原至新的服務實例，請遵循步驟來檢視現有備份，然後按一下對應列以展開您要下載之備份的選項。按一下**還原**按鈕。即會顯示一則訊息，讓您知道已起始還原。新的服務實例會自動命名為 "janusgraph-restore-[timestamp]"，而且在佈建開始時，儀表板上會出現這個實例。

### 透過 {{site.data.keyword.cloud_notm}} CLI 還原

請使用下列步驟，以使用 {{site.data.keyword.cloud_notm}} CLI 將備份從執行中 JanusGraph 服務還原至新的 JanusGraph 服務。 
1. 如果您需要，請[下載並安裝它](https://console.{DomainName}/docs/cli/index.html#overview)。 
2. 在服務的_備份_ 頁面上，尋找您要從中還原的備份，然後複製備份 ID。  
  **或**  
  透過 Compose API 使用 `GET /2016-07/deployments/:id/backups` 尋找備份及其 ID。「基礎端點」及服務實例 ID 都會顯示在服務的_概觀_ 中。例如： 
  ``` 
  https://composebroker-dashboard-public.mybluemix.net/api/2016-07/instances/$INSTANCE_ID/deployments/$DEPLOYMENT_ID/backups
  ```  
  回應包含該服務實例之所有可用備份的清單。請挑選您要從中還原的備份，並複製其 ID。

3. 使用適當的帳戶及認證登入。`ibmcloud login`（或 `ibmcloud login -help` 以查看所有登入選項）。

4. 切換至「組織」及「空間」：`ibmcloud target -o "$YOUR_ORG" -s "YOUR_SPACE"`

5. 使用 `service create` 指令來佈建新的服務，並在 JSON 物件中提供您要還原的來源服務及特定備份。例如：
``` 
ibmcloud service create SERVICE PLAN SERVICE_INSTANCE_NAME -c '{"source_service_instance_id": "$SERVICE_INSTANCE_ID", "backup_id": "$BACKUP_ID" }'
```
  對於 _SERVICE_ 請使用 `compose-for-janusgraph` 的值，對於 _PLAN_ 請使用 Standard 或 Enterprise，視您的環境而定。_SERVICE\_INSTANCE\_NAME_ 是您放置新服務名稱的位置。_source\_service\_instance\_id_ 是備份來源的服務實例 ID；其取得方式是執行 `ibmcloud cf service DISPLAY_NAME --guid`，其中 _DISPLAY\_NAME_ 是備份來源的 JanusGraph 服務名稱。 
  
  如果您是企業使用者，則也需要使用 `"cluster_id": "$CLUSTER_ID"` 參數，在 JSON 物件中指定要部署到哪個叢集。
  
### 升級至新版本

現行執行中部署無法進行部分主要版本升級。您需要佈建執行已升級版本的新服務，然後使用備份將您的資料移轉至其中。此處理程序與還原備份的處理程序相同，但在這裡您會指定要升級到的版本。

``` 
ibmcloud service create SERVICE PLAN SERVICE_INSTANCE_NAME -c '{"source_service_instance_id": "$SERVICE_INSTANCE_ID", "backup_id": ""$BACKUP_ID", "db_version":"$VERSION_NUMBER" }'
```

例如，將舊版的 {{site.data.keyword.composeForJanusGraph}} 服務還原至執行 JanusGraph 0.1.1 的新服務，看起來如下：
```
ibmcloud service create compose-for-janusgraph Standard migrated_janusgraph -c '{ "source_service_instance_id": "0269e284-dcac-4618-89a7-f79e3f1cea6a", "backup_id":"5a96d8a7e16c090018884566", "db_version":"0.1.1"  }'
