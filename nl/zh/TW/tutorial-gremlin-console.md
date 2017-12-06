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

# 使用 Gremlin 主控台建立及遍訪圖形

本主題將協助您開始使用您的第一個 {{site.data.keyword.composeForJanusGraph_full}} 資料庫。針對此指導教學，我們將使用 Gremlin 主控台來傳送指令給 JanusGraph 伺服器。

## 1. 安裝 Gremlin

若要安裝 Gremlin 主控台，請執行下列動作：

1. 確定您已安裝最新版本的 Java。
2. 下載 [Gremlin 主控台 3.2.3 版](https://archive.apache.org/dist/tinkerpop/3.2.3/apache-tinkerpop-gremlin-console-3.2.3-bin.zip)。
3. 將下載的檔案解壓縮到工作目錄。
4. 使用指令行或終端機，切換目錄至 Gremlin 主控台根目錄，然後執行 `bin/gremlin.sh` 驗證下載：

  ```text
  $ unzip -q  ~/Downloads/apache-tinkerpop-gremlin-console-3.2.3-bin.zip
  $ cd apache-tinkerpop-gremlin-console-3.2.3
  $ bin/gremlin.sh

          \,,,/
          (o o)
  -----oOOo-(3)-oOOo-----
  plugin activated: tinkerpop.server
  plugin activated: tinkerpop.utilities
  plugin activated: tinkerpop.tinkergraph
  gremlin> [CONTROL-D]                                                             $

  ```

5. 配置 Gremlin 主控台。您可以在 {{site.data.keyword.composeForJanusGraph}} 服務的*概觀* 頁面找到 Gremlin 主控台 YAML。將其中一個配置儲存成 `conf` 目錄中的檔案。在此範例中，我們將它儲存為 `conf/compose.yaml`。
 
## 2. 連接至 Compose for JanusGraph

若要驗證連線，請再次執行 `bin/gremlin.sh`。然後使用 `:remote` 指令，以連接至 tinkerpop 伺服器：

```text
:remote connect tinkerpop.server conf/compose.yaml session
```

這個指令會告訴 Gremlin `connect` 至 `tinkerpop.server` 的某個東西，並使用 `conf/compose.yaml` 中的配置。我們也傳遞了另一個引數 - `session` - 以啟用階段作業支援。

鍵入 `:help :remote` 以查看 `:remote` 指令的選項。
{: .tip}

如果連線成功，您將會看到 SSL 警告以及確認連線的訊息。

```text
WARN  org.apache.tinkerpop.gremlin.driver.Cluster  - SSL configured without a trustCertChainFile and thus trusts all certificates without verification (not suitable for production)
==>Configured portal93-4.ianuspater.compose-3.composedb.com/159.8.153.216:18045-[2378d0de-0a93-4330-b8d8-848667f5b117]
gremlin>
```

## 3. 傳送指令至伺服器。

Gremlin 主控台不會自動將任何東西轉遞至遠端伺服器：若要傳送指令給伺服器，您需要為指令加上 `:>` 字首。或者，您可以使用 `:remote console`，將所有主控台指令重新導向至遠端伺服器。這點藉由下列系列的指令及回應來示範：

```text
$ bin/gremlin.sh                                                                   

        \,,,/
        (o o)
-----oOOo-(3)-oOOo-----
plugin activated: tinkerpop.server
plugin activated: tinkerpop.utilities
plugin activated: tinkerpop.tinkergraph
gremlin> 1+1
==>2
gremlin> :> 1+1
==>No remotes are configured.  Use :remote command.
gremlin> :remote connect tinkerpop.server conf/compose.yaml session
WARN  org.apache.tinkerpop.gremlin.driver.Cluster  - SSL configured without a trustCertChainFile and thus trusts all certificates without verification (not suitable for production)
==>Configured portal93-4.ianuspater.compose-3.composedb.com/159.8.153.216:18045-[016d7a68-cf70-450e-92eb-4e5e2a647b5b]
gremlin> :remote console
==>All scripts will now be sent to Gremlin Server - [portal93-4.ianuspater.compose-3.composedb.com/159.8.153.216:18045]-[016d7a68-cf70-450e-92eb-4e5e2a647b5b] - type ':remote console' to return to local mode
gremlin> 1+1
==>2
gremlin> 

```

## 4. 建立圖形

{{site.data.keyword.composeForJanusGraph}} 有專用的圖形 Factory，可以建立、開啟及關閉圖形。使用此 Factory - `ConfiguredGraphFactory` - 表示您不需要知道基礎儲存機制，因此建立新的圖形只不過是為它命名這麼回事。請由建立新圖形開始。在本指導教學的範例指令中，我們會將圖形稱為 _mygraph_。

請使用 `def` 關鍵字來宣告 graph 變數。

```
def graph=ConfiguredGraphFactory.create("mygraph")
```

{{site.data.keyword.composeForJanusGraph}} 圖形名稱只能包含英數字元和底線字元。
{: .tip}

## 5. 開啟圖形

建立圖形之後，如果要使用圖形，則需要開啟圖形。方法是對 `ConfiguredGraphFactory` 呼叫 `open()`。讓我們開啟我們的圖形：

```
def graph=ConfiguredGraphFactory.open("mygraph")
```

在這裡，我們需要確定作業，以便圖形能持續保存：

```
graph.tx().commit()
```

如果現在關閉我們的階段作業，新的圖形會在我們返回時出現。

## 6. 將頂點新增至圖形

我們的圖形將包含軟體開發人員及他們已開發之軟體部分的相關資訊。讓我們新增一個叫做 "marko" 的人，以及一個稱為 "lop" 的軟體。

首先，讓我們新增 marko。

```
def v1 = graph.addVertex(T.label, "person", "name", "marko", "age", 29)
```

如果成功，這個指令將傳回我們已新增之頂點的索引 - `==>v[4304]` - 然後我們可以繼續新增第二個頂點。

```
def v2 = graph.addVertex(T.label, "software", "name", "lop", "lang", "java")
```

之後，我們需要確定這些作業。

```
graph.tx().commit()
```

## 7. 在圖形中尋找頂點

若要找到頂點，假設是名稱為 "marko" 的頂點，我們要先遍訪圖形，然後使用 has() 方法，它會傳回頂點的 ID。

```
def g=graph.traversal()
g.V().has("name","marko")
```

為了將我們的頂點重新指派給變數 v1 和 v2，假設我們已關閉舊的階段作業並開始新的階段作業，我們使用 next()。

```
def v1 = g.V().has("name","marko").next()
def v2 = g.V().has("name","lop").next()
```

然後我們可以在後續的 Gremlin 指令裡使用這些變數。

## 8. 新增邊緣

接下來，我們可以在兩個頂點之間新增加權的邊緣，並賦與邊緣 "created" 的標籤，以及權重 0.4。

請注意，您需要將數字強制轉型為小數，方法是將值指定為 `0.4d`。

```
v1.addEdge("created", v2, "weight", 0.4d)
```

同樣地，請確定您已確定變更。

## 9. 查詢圖形

您現在可以查詢圖形，以找到 Marko 已建立的任何軟體的名稱。

```
g.V().has("name","marko").out("created").values("name")
```
