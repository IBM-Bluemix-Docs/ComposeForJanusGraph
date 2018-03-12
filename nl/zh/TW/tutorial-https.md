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

# 使用 HTTPS 建立及遍訪圖形

{{site.data.keyword.composeForJanusGraph_full}} 隨附了範例圖形資料庫，即 'The Graph of the Gods'。本指導教學使用該個範例資料庫來說明一些 JanusGraph 概念。請遵循步驟來建立、開啟及遍訪圖形，並使用 CURL 來傳送 Gremlin 指令。若要檢視圖形的視覺化表示法，請參閱 JanusGraph [Getting Started](http://docs.janusgraph.org/latest/getting-started.html) 文件。 

## 1. 連接至 Compose for JanusGraph

若要使用 CURL 連接至您的 {{site.data.keyword.composeForJanusGraph}} 服務，您需要使用連線字串。連線字串包含連接至服務所需的使用者名稱、密碼、伺服器及埠號。您可以在服務儀表板的_概觀_ 頁面找到。

在開始指導教學之前，請驗證您可以使用連線字串進行連接，方法是傳遞簡單的 Gremlin 指令：

```
curl -XPOST -d '{"gremlin" : "1+1" }' "<CONNECTION STRING>"
```

如果要求成功，則 JanusGraph 會傳回如下的回應：

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

您現在可以開始使用 The Graph of the Gods。

將您的連線字串新增至指導教學中每個 CURL 指令的尾端。
{: .tip}

## 2. 建立圖形

{{site.data.keyword.composeForJanusGraph}} 有專用的圖形 Factory，可以建立、開啟及關閉圖形。使用此 Factory - `ConfiguredGraphFactory` - 表示您不需要知道基礎儲存機制，因此建立新的圖形只不過是為它命名這麼回事。請由建立新圖形開始，稱為 _Example_。

JanusGraph 沙盤推演需要您宣告要使用的所有變數。若要宣告變數，請使用 `def` 關鍵字。例如，要宣告 graph 變數：

```
curl -XPOST -d '{"gremlin": "def graph=ConfiguredGraphFactory.create(\"example\");0;"}'
```

{{site.data.keyword.composeForJanusGraph}} 圖形名稱只能包含英數字元和底線字元。
{: .tip}

請注意，curl 指令會以 `;0;` 結束。這是一個暫行解決方法，因為 HTTP 要求 API 會發出錯誤 (`{"message":"Cannot get namespace of root","Exception-Class":"java.lang.IllegalArgumentException"}`)，即使它已正確地完成作業。這只會在程式碼嘗試傳回時影響圖形類型。

## 3. 開啟圖形

建立圖形之後，如果要使用圖形，則需要開啟圖形。方法是對 `JanusGraphConfiguredFactory` 呼叫 `open()`。讓我們開啟我們的 'example' 圖形。

```
curl -XPOST -d '{"gremlin": "def graph=ConfiguredGraphFactory.open(\"example\");0;"}'
```

## 4. 載入 The Graph of the Gods

若要載入範例圖形，請呼叫 `GraphOfTheGods.loadWithoutMixedIndex()` 方法，並傳遞已開啟的 'example' 圖形給它，如此便會為您建立模型。

```
curl -XPOST -d '{"gremlin": "def graph=ConfiguredGraphFactory.open(\"example\"); GraphOfTheGodsFactory.loadWithoutMixedIndex(graph,true);"}'
```

## 5. 準備遍訪圖形

現在，您已建立並開啟圖形，並已在其中載入一些資料，您可以探索圖形。若要四處移動並查詢圖形，您需要一個遍訪來源，這是藉由對圖形物件呼叫 `traversal()` 而取得。這表示任何指令的完整字首會是：

```
curl -XPOST -d '{"gremlin": "def graph=ConfiguredGraphFactory.open(\"example\");def g=graph.traversal();0;"}'
```

常見針對圖形使用 `graph`，以及針對遍訪來源使用 `g`。如果您只需要遍訪來源，可以將這壓縮為：

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal();0;"}'
```

## 6. 遍訪圖形以尋找 Saturn

The Graph of the Gods 圖形包含 name 內容的廣域索引。這種索引經常是您到達圖形中特定點的第一步。例如，若要在圖形中找到 "saturn"：

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next()"}'
```

此指令傳回具有內容 `name: saturn` 的頂點。

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

## 7. 遍訪圖形以尋找 Saturn 的子項

進一步使用這個範例，藉由在變數 'saturn' 中定義它來使用 "saturn" 頂點，您便可以詢問哪些頂點儲存了具有 'father' 標籤指向它的邊緣。使用 `in("father")` 方法會選取那些送入的邊緣，而使用 `values("name")` 會傳回該頂點 name 內容的值。

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); ;g.V(saturn).in(\"father\").values(\"name\")"}'
```

查詢會傳回圖形中 Saturn 子項的名稱，不過只有一個子項 "jupiter"。

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

如果您第二次包含 `in("father")` 作業，則遍訪會將您帶到 Saturn 頂點的孫項。

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); ;g.V(saturn).in(\"father\").in(\"father\").values(\"name\")"}'
```

使用此查詢，您發現 Saturn 孫項的名稱是 'hercules'。

## 8. 遍訪圖形 - 位置

在 GraphOfTheGods 中，有些邊緣有 "place" 內容，包含了緯度和經度。這個內容可以用於地理定位事件。標籤為 "battled" 的邊緣使用 place 內容來代表戰役發生之處。若要找出在雅典（緯度：37.97，經度 23.72）50 公里內發生的所有戰役，請在圖形的邊緣中，搜尋落在那些座標半徑內的任何 "place" 內容值。

您可以使用 `has("place", geoWithin(Geoshape.circle(37.97, 23.72, 50)))` 選取那些邊緣。

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); g.E().has(\"place\", geoWithin(Geoshape.circle(37.97, 23.72, 50)))"}'
```

查詢會傳回送入及送出頂點的邊緣標籤，以及它們的 ID。為了使回應能更立即可讀，以及找出誰參與了這兩場戰役，我們可以使用 `as()` 將傳回的值隱藏為 _god1_ 及 _god2_，然後使用 `select()` 擷取那些值，並使用 `by("name")` 擷取每場戰役中牽涉的兩位神祇名稱。

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

## 9. 遍訪圖形 - 頂點

在稍早的步驟中，您使用了兩個 `in()` 元素來識別 Hercules 是 Saturn 之孫。遍訪也可以表達為迴圈，因為 Gremlin 可以 `repeat` 述詞：

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); def hercules=g.V(saturn).repeat(__.in(\"father\")).times(2).next()"}'
```

您可以執行使用 Hercules 頂點作為起點的進階作業。例如，您可以：

- 遍訪標籤為 "father" 和 "mother" 的邊緣，確定 Hercules 的雙親的類型 "type"：

  ```
  curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); def hercules=g.V(saturn).repeat(__.in(\"father\")).times(2).next(); g.V(hercules).out(\"father\", \"mother\").label();" }'
  ```

- 遍訪 "battled" 邊緣以查看與 Hercules 對戰者：

  ```
  curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); def hercules=g.V(saturn).repeat(__.in(\"father\")).times(2).next(); g.V(hercules).out(\"battled\").valueMap()" }' 
  ```

- 遍訪 "battled" 邊緣以多次查看與 Hercules 對戰者：

  ```
  curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); def hercules=g.V(saturn).repeat(__.in(\"father\")).times(2).next(); g.V(hercules).outE(\"battled\").has(\"time\", gt(1)).inV().values(\"name\")" }' 
  ```
  
