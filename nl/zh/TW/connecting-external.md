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

# 連接至 JanusGraph

JanusGraph 支援使用 HTTP 要求或透過 WebSocket 的連線。所有連線都是透過 HTTPS 來執行，且需要鑑別。HTTP 型要求支援基本鑑別及記號鑑別。對於 JanusGraph 部署上一次性呼叫以外的任何其他項目，使用 WebSocket 或「記號鑑別」可讓效能較佳。基本鑑別是一種減慢要求速度的昂貴處理程序。

如需如何使用 HTTP，以及瞭解如何建立及開啟圖形的相關資訊，請參閱[使用 HTTPS 建立及遍訪圖形](./tutorial-https.html)。
{: .tip}

用戶端與資料庫之間的實際通訊包括用戶端傳送要求（在 Gremlin 中），這是 Groovy 語言的自訂用語，用於查詢圖形結構及回應那些查詢的資料庫。您也可以使用「Gremlin 主控台」來傳送要求。

如需如何取得、安裝、配置及使用「Gremlin 主控台」的詳細資料，請參閱[使用 Gremlin 主控台建立及遍訪圖形](./tutorial-gremlin-console.html)。
{: .tip}

## HTTP 要求

如果您要使用「HTTP 要求」來連接至伺服器，則單一端點可用來與 JanuGraph 通訊，並設為在部署中所有可用的節點之間循環。其中包括主機名稱和埠。

您可以從服務儀表板的概觀頁面取得「HTTPS 連線字串」。連線字串需要使用管理使用者認證進行基本鑑別，才能連接至伺服器。

若要使用 HTTP 要求連接至 {{site.data.keyword.composeForJanusGraph}}，您需要使用 HTTP POST 方法與端點搭配，並傳送具有單一項目（內含索引鍵 "gremlin" 以及您要傳送之 Gremlin 查詢的值）的 JSON 格式文件。 

```json
{
    "gremlin": "a gremlin query would go here"
}
```

Gremlin 是一種廣泛使用的遍訪語言，但其中的程式也可以像 "1+1" 一樣簡單。若要在 `curl` 指令中將程式當作 Script 傳遞 ，POST 要求的資料部分如下：

```
'{"gremlin" : "1+1" }'
``` 

如果您要將相同的 Script 傳送至伺服器數次，您可以提供選用的「連結」字典，並提供 Gremlin Script 可使用的變數對映，以增進效能。這有助於提高效能，因為伺服器不需要重新編譯 Gremlin Script。
{: .tip}

如需 HTTP 通訊協定的相關資訊，請參閱「Apache Tinkerpop 文件」的[透過 REST 連接](http://tinkerpop.apache.org/docs/3.2.3/reference/#_connecting_via_rest)小節

若要將 Gremlin Script 傳送至端點，您需要進行鑑別。有兩種方法可使用 HTTP 要求存取進行鑑別：基本鑑別和記號鑑別。

## 基本鑑別

基本鑑別會藉由要求傳送您的使用者名稱及密碼。若為一次性查詢，它是合理的，但在多次查詢時，您開始載入伺服器，因而帶來執行密碼交換及驗證的複雜性。您可以使用 curl 與 `-u` 選項搭配來傳遞使用者名稱及密碼：

```shell
$ curl -XPOST -d '{"gremlin" : "1+1" }' "https://portal91-0.ianuspater.compose-3.composedb.com:17916" -u admin:PASSWORDHERE
```

您也可以在端點 URL 中內含使用者名稱及密碼。 

## 記號鑑別

如需更有效率的 HTTP 要求，您可以從另一個端點取得階段作業記號。此記號會在標頭中傳遞給所有後續要求。如果您正在 HTTP 端點上執行多次呼叫，則可以使用記號鑑別來增進效能。

請使用服務儀表板概觀頁面的_階段作業_ 畫面中的任一個連線字串。連線字串已包括以 curl 指令取得記號所需的呼叫：

```shell
$ curl -XGET "https://admin:PASSWORDHERE@portal91-0.ianuspater.compose-3.composedb.com:17916/session"
```

傳回的 JSON 結果包含階段作業記號：

```
{
  "token": "YWRtaW46MTQ5NjkyODM1NDgzMzpWYXlxMERsMWs0VmltdGo3WUpzMEwtN3prUi00TGxZR3J6LXZnbDVmN3lnPQ=="
}
```

記號有效時間為 60 分鐘
{: .tip}

擷取記號值，然後在任何進一步要求中包括它，作為 `Authorization` 標頭：

```shell
$ curl -XPOST -H "Authorization: Token YWRtaW46MTQ5NjkyODM1NDgzMzpWYXlxMERsMWs0VmltdGo3WUpzMEwtN3prUi00TGxZR3J6LXZnbDVmN3lnPQ==" -d '{"gremlin" : "1+1" }' "https://portal91-0.ianuspater.compose-3.composedb.com:17916"
```

如果您要從 Shell 執行此動作，且有 [`jq`](https://stedolan.github.io/jq/) 指令可供使用，則可以執行如下指令設定環境變數：

```shell
$ export JGTOKEN=`curl -s -XGET "https://admin:PASSWORDHERE@portal91-0.ianuspater.compose-3.composedb.com:17916/session" | jq -r .token`
```

然後，執行如下要求：

```shell
$ curl -XPOST -H "Authorization: Token $JGTOKEN" -d '{"gremlin" : "1+1" }' "https://portal91-0.ianuspater.compose-3.composedb.com:17916"
```

## HTTP JSON 回應

對查詢的回應遵循具有要求 ID 值、狀態物件及結果物件的 JSON 文件格式。以下是回應的範例：

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
`requestId` 是要求的唯一 ID。`status` 包含可讀取的訊息、HTTP 狀態樣式碼，以及報告狀態的其他屬性。`result` 包含一個資料物件，其結構根據執行的 Gremp 程式碼所傳回的任何內容，以及包含一個 meta 區段，其中有超出結果資料範圍外的所有額外資訊。

## Websocket

WebSocket 是透過 TCP 及 TLS 建立長期存活的雙向連線的有效方式，此連線很適合用戶端應用程式與 JanusGraph 伺服器之間的互動式階段作業。一些 JanusGraph 程式庫會使用 WebSocket 作為其與伺服器的連線；若要在 Compose 上使用 JanuGraph，它們必須能夠執行基本鑑別，以及使用 WSS（透過 TLS 的安全 WebSocket）。 

您可以從服務儀表板概觀頁面的 _Websocket_ 區段中，取得「Websocket 連線字串」。URI 可以用來建立搭配 JanusGraph 部署的長時間執行階段作業，並以 `wss:` 作為字首，表示連線會以 HTTPS 保護。這些連線字串需要基本鑑別與管理使用者認證搭配使用，才能連接至伺服器。

## Gremlin 主控台

「Gremlin 主控台」是一種基本工具，用於使用已啟用 Tinkerpop 的伺服器。「Gremlin 主控台」使用 WebSocket 連線功能，但不會使用 URI 語法進行連接；部分原因是它也需要配置其他元素來使用連線。

若要配置「Gremlin 主控台」，請提供其中具有適當詳細資料的 YAML 檔案。您可以在服務儀表板概觀頁面的 _Gremlin 主控台 YAML_ 畫面中找到此配置。

如需如何取得、安裝、配置及使用「Gremlin 主控台」的詳細資料，請參閱[使用 Gremlin 主控台建立及遍訪圖形](./tutorial-gremlin-console.html)。
