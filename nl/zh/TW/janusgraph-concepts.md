---

copyright:
  years: 2017,2018
lastupdated: "2017-12-12"
---

# JanusGraph 概念

JanusGraph 是圖形資料庫。您可以在資料庫中建立、查詢及修正許多圖形。JanusGraph 建置於 Apache Tinkerpop 堆疊之上，並使用 Gremlin 語言來進行遍訪、指令及查詢。

若要閱讀 JanusGraph 的相關資訊，請參閱 [JanusGraph 文件](http://docs.janusgraph.org/latest/index.html)。

<iframe title="Compose for JanusGraph 概觀" class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="//www.youtube.com/embed/zTaoMWv6lnE?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

您可以在 [IBM Compose for JanusGraph Learning Center](http://ibm.biz/janusgraph-learning) 找到其他關於 {{site.data.keyword.composeForJanusGraph}} 的視訊。
{: .tip}

## 圖形資料庫簡介

最簡單來說，圖形是一組頂點，並且在頂點之間有邊緣。頂點是圖形的基礎單位，並代表部分個別物件。實際上，這可能是人員、位置或物件。它可以有內容來說明物件。 

邊緣是兩個頂點之間的連線，表示它們之間的關係。它可以具有對應關係、方向及內容。

在非圖形資料庫中，實體之間的關係為資料庫的次要功能，它們是透過欄位中的共用索引鍵來表示，或透過 JOIN 建立。這表示下列關係可能需要多個查詢或應用程式執行從更多一般查詢組合關係的工作。

在圖形資料庫中，關係是資料模型的主要元件，而在模型內查詢資料的主要機制則是從頂點至邊緣遍訪至頂點及以外之處。那樣會使得圖形資料庫更適合牽涉到關係的查詢，關係的範例諸如「喜歡品牌 X 且有朋友（或朋友的朋友）喜歡品牌 Y 的所有人」。 

在 JanusGraph 內，每個圖形都有唯一名稱。若要建立、開啟、修改及查詢圖形，請使用 Gremlin 語言。Gremlin 查詢包含可以操作或遍訪圖形的指令，這些指令會以某種方式提供您想要的結果。

## 頂點

頂點是代表物件的圖形元素。在頂點建立時，它有一個由圖形引擎設定的 ID，以及一個使用者定義的標籤。頂點可以額外儲存可以編製索引與用來查詢的內容。

若要閱讀如何建立、查詢頂點的相關資訊，以及其他實作詳細資料，請參閱 [Tinkerpop/Gremlin 文件](http://tinkerpop.apache.org/docs/3.2.3/reference/#_the_graph_structure)。

## 邊緣

邊緣是代表關係的圖形元素。若要建立邊緣，使用者要提供送入/送出的頂點與標籤。將由圖形引擎指派 ID 給邊緣。邊緣可以額外儲存可以編製索引與用來查詢的內容。

若要閱讀如何建立、查詢邊緣的相關資訊，以及其他實作詳細資料，請參閱 [Tinkerpop/Gremlin 文件](http://tinkerpop.apache.org/docs/3.2.3/reference/#_the_graph_structure)。

## 內容

內容是 key:value 配對，它會說明、命名邊緣或頂點，或是與其相關聯。邊緣及頂點都可能有多個內容。例如，如果頂點的類型為 'human'，我們可以指派內容 'name' 及 'age' 給該頂點。

內容可以加以編製索引，以便能依內容擷取頂點和邊緣，例如取得具有相同名稱的所有頂點。

邊緣的內容可能像是 'weight'，因此透過多個加權方式不同的邊緣進行頂點的圖形遍訪，可能會得到旅程的計算總權重。 

## 遍訪

遍訪是分析圖形結構的過程。遍訪的過程會探索並傳回邊緣、頂點及其內容的相關資訊。若要瞭解不同的遍訪步驟與演算法，請參閱 [關於遍訪的 Tinkerpop/Gremlin 文件](http://tinkerpop.apache.org/docs/3.2.3/reference/#traversal)。

## Gremlin

Gremlin 有兩部分：語言及伺服器。

Gremlin 語言是您用來互動及查詢圖形的圖形遍訪語言。

Gremlin 伺服器則是處理本端或遠端 Gremlin 語言查詢的伺服器規格。其實作屬於 Apache Tinkerpop 的一部分。

若要執行 Gremlin 語言查詢，請將 Gremlin 查詢傳送至適當的 Gremlin 伺服器。在 Compose 上，那是在您的 JanusGraph 部署當中執行的伺服器。

## Apache TinkerPop

Apache TinkerPop 是開放程式碼的「圖形運算架構」。TinkerPop 系統會將資料塑模為圖形，並與 Gremlin 語言指令互動，以建立、遍訪及操作圖形。若要閱讀這些部分如何合為一體的相關資訊，請參閱關於[圖形系統整合](http://tinkerpop.apache.org/docs/3.2.3/reference/#_graph_system_integration)的 TinkerPop/Gremlin 文件。
