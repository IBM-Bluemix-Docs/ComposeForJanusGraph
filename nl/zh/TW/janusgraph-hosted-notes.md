---
copyright:
  years: '2015, 2017'
lastupdated: '2017-11-21'
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# JanusGraph Hosted 注意事項

{{site.data.keyword.composeForJanusGraph_full}} 是一種托管服務，因此在少數情況下，確保部署安全與專用伺服器上執行的 JanusGraph 不同。本主題強調那些差異與差別。這些變更大部分會影響 Gremlin 語言查詢的運作方式。

## 沙盤推演

為了確保 Gremlin 查詢不會存取可能危及系統的功能，所有查詢都會在沙盤推演中執行。這表示，所有函數及方法呼叫都必須具有符合下列其中一個正規表示式的簽章：

```
java\.util.*
java\.lang\.Object.*
java\.lang\.Class <T extends java\.lang\.Object>#getSimpleName\(
java\.lang\.Iterable <T extends java\.lang\.Object>#iterator\(\)
java\.lang\.Boolean(?!#getBoolean\().*
java\.lang\.Double.*
java\.lang\.Float.*
java\.lang\.Integer(?!#getInteger\().*
java\.lang\.Long(?!#getLong\().*
java\.lang\.Math.*
java\.lang\.String(?!#intern\(\)).*
java\.lang\.StringBuilder.*
java\.lang\.Object#equals\(
java\.lang\.Object#hashCode\(
java\.util\.ArrayList#equals\(

org\.codehaus\.groovy\.runtime\.DefaultGroovyMethods.*
org\.codehaus\.groovy\.runtime\.StringGroovyMethods.*
org\.apache\.commons\.configuration\.MapConfiguration.*
org\.apache\.tinkerpop\.gremlin\.structure(?!\.io|\.Graph#toString\(|\.Graph#variables\(|\.Graph#configuration\(|\.Graph#io\(\)|\.util\.GraphFactory.*).*
org\.apache\.tinkerpop\.gremlin\.process\.traversal.*
org\.apache\.tinkerpop\.gremlin\.util\.Gremlin#version\(\)
org\.janusgraph\.core(?!\.JanusGraphFactory.*).*
org\.janusgraph\.graphdb(?!\.tinkerpop\.JanusGraphBlueprintsGraph#toString\(|\.tinkerpop\.JanusGraphBlueprintsGraph#variables\(|\.tinkerpop\.JanusGraphBlueprintsGraph#configuration\(|\.tinkerpop\.JanusGraphBlueprintsGraph#io\().*
org\.janusgraph\.example\.GraphOfTheGodsFactory(?!#create\(|#main\().*
org\.apache\.tinkerpop\.gremlin\.groovy\.plugin\.dsl\.credential\.CredentialGraph.*

Script\d+#.+\(?.+\)
groovy\.lang\.Closure <V extends java\.lang\.Object>#call\(?.+\)

org\.janusgraph\.util\.stats\.MetricManager#getRegistry\(\)
org\.janusgraph\.util\.stats\.MetricManager#INSTANCE
org\.apache\.tinkerpop\.gremlin\.util\.MetricManager#getRegistry\(\)
org\.apache\.tinkerpop\.gremlin\.util\.MetricManager#INSTANCE
```

嘗試執行的函數或方法呼叫，若不符合函數白名單中的任何項目，將產生錯誤： 

```
[Static type checking] - Not authorized to call this method: ...
```

沙盤推演也會要求所有變數進行靜態宣告，以啟用安全檢查。您可以利用 `def` 指令來執行此動作。

## 階段作業

階段作業容許連線至 {{site.data.keyword.composeForJanusGraph}}，以維護要求之間的狀態。{{site.data.keyword.composeForJanusGraph}} 的大部分連線選項沒有與它們相關聯的階段作業。這表示，透過那些連線方法傳送的任何 Gremlin Script 需要整個自行包含。例如，開啟 Script 需要使用的任何圖形、查詢適當的節點，以及從那些節點中遍訪圖形，全都需要在一個要求中進行處理。此限制同時適用於 HTTP 要求及 WebSockets 連線。 

當您使用 `:remote connect` 從 Gremlin 主控台進行連接時，不受此限。

```
:remote connect tinkerpop.server conf/compose.yaml
```

如果使用上述指令進行連線，則沒有階段作業，而且連線會採取與 HTTP 要求相同的方式運作。但是，如果新增 `session` 作為引數，則會立即啟用階段作業支援。

```
:remote connect tinkerpop.server conf/compose.yaml session
```

使用階段作業，您可以在一個指令中定義變數，並在另一個指令中參照它。
{: .tip}

## 交易

基礎 JanusGraph 資料庫的所有變更都會封裝在交易中。交易容許對它們之內的任何變更進行確定，使它們成為永久變更，或回復以確保不會變更。 

當透過 HTTP 要求或 WebSocket 提出要求時，若您進行一項將寫入至資料庫的變更，則交易會自動啟動。當要求完成時，也會自動確定該交易。

使用已啟用階段作業的「Gremlin 主控台」進行連接時，不在此限。階段作業可以長期存在，因此必須由使用者呼叫 `graph.tx().commit()` 來確定任何變更，其中 `graph` 是包含變更的已開啟圖形。這也表示您可以呼叫 `graph.tx().rollback()`，回到交易開始之前的圖形狀態。 

## 混合索引

{{site.data.keyword.composeForJanusGraph}} 的現行版本不支援混合索引。
