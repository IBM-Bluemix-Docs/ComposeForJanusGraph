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

# HTTPS を使用したグラフの作成とトラバース

{{site.data.keyword.composeForJanusGraph_full}} には、「The Graph of the Gods」というサンプル・グラフ・データベースが用意されています。 このチュートリアルでは、JanusGraph の概念を理解するためにそのサンプル・データベースを使用します。 グラフを作成したり開いたりトラバースしたりする手順を実行するために、CURL を使用して Gremlin コマンドを送信してください。 グラフのビジュアル表示を確認したい場合は、JanusGraph の[入門資料](http://docs.janusgraph.org/latest/getting-started.html)を参照してください。 

## 1. Compose for JanusGraph に接続します。

CURL を使用して {{site.data.keyword.composeForJanusGraph}} サービスに接続する時は、接続ストリングを使用する必要があります。 接続ストリングには、サービスへの接続に必要なユーザー名、パスワード、サーバー、ポート番号が入っています。 サービス・ダッシュボードの_「概要」_ページにその情報があります。

このチュートリアルを開始する前に、簡単な Gremlin コマンドを渡して、接続ストリングで接続できることを確認してください。

```
curl -XPOST -d '{"gremlin" : "1+1" }' "<CONNECTION STRING>"
```

要求が成功すると、JanusGraph から以下のような応答が返されます。

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

これで、「The Graph of the Gods」の操作を開始する準備が整いました。

このチュートリアルでは、各 CURL コマンドの最後に接続ストリングを追加してください。
{: .tip}

## 2. グラフを作成します。

{{site.data.keyword.composeForJanusGraph}} には、グラフを作成したり開いたり閉じたりするための専用のグラフ・ファクトリーがあります。 このファクトリー (`ConfiguredGraphFactory`) を使用すれば、基礎になっているストレージ・メカニズムについて知る必要はありません。グラフに名前を付けるだけで、新しいグラフを作成できます。 最初に、_example_ という新しいグラフを作成してください。

JanusGraph サンドボックスでは、使用するすべての変数をまず宣言する必要があります。 変数の宣言では、`def` キーワードを使用します。 例えば、以下のようにして graph 変数を宣言します。

```
curl -XPOST -d '{"gremlin": "def graph=ConfiguredGraphFactory.create(\"example\");0;"}'
```

{{site.data.keyword.composeForJanusGraph}} のグラフ名には、英数字と下線文字しか使用できません。
{: .tip}

この curl コマンドでは、最後に `;0;` を付けています。 このような回避策を使用するのは、操作が正常に完了した場合でも、HTTP 要求の API から (`{"message":"Cannot get namespace of root","Exception-Class":"java.lang.IllegalArgumentException"}`) というエラーが出されるからです。 この影響を受けるのは、コードによってグラフ・タイプを返す場合に限られます。

## 3. グラフを開きます。

グラフを作成したら、グラフを操作するためにまず開く必要があります。 そのために、`JanusGraphConfiguredFactory` で `open()` を呼び出します。 それでは、次のように example グラフを開いてください。

```
curl -XPOST -d '{"gremlin": "def graph=ConfiguredGraphFactory.open(\"example\");0;"}'
```

## 4. 「The Graph of the Gods」をロードします。

サンプル・グラフをロードするために、`GraphOfTheGods.loadWithoutMixedIndex()` メソッドを呼び出して、開いた example グラフに渡します。これで、モデルが作成されます。

```
curl -XPOST -d '{"gremlin": "def graph=ConfiguredGraphFactory.open(\"example\"); GraphOfTheGodsFactory.loadWithoutMixedIndex(graph,true);"}'
```

## 5. グラフのトラバースの準備をします。

グラフを作成し、開いて、グラフにデータをロードしたので、グラフを探索できる状態になりました。 グラフ内を移動して照会を実行するには、トラバーサル・ソースが必要なので、グラフ・オブジェクトに対して `traversal()` を呼び出してそのソースを取得します。 どのコマンドを実行する場合でも、以下の接頭部全体を使用します。

```
curl -XPOST -d '{"gremlin": "def graph=ConfiguredGraphFactory.open(\"example\");def g=graph.traversal();0;"}'
```

一般的なのは、グラフに `graph` を使用し、トラバーサル・ソースに `g` を使用するという方法です。 トラバーサル・ソースだけが必要な場合は、以下のように圧縮できます。

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal();0;"}'
```

## 6. グラフをトラバースして saturn を見つけます。

「The Graph of the Gods」グラフには、name プロパティーのグローバル索引が付いています。 多くの場合、この種の索引が、グラフ内の特定のポイントにたどり着くための最初のステップになります。 例えば、このグラフで saturn を検索するには、以下のようにします。

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next()"}'
```

このコマンドを実行すると、`name: saturn` というプロパティーを持った頂点が返されます。

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

## 7. グラフをトラバースして Saturn の子を見つけます。

このサンプルの操作をさらに進めるために、saturn という頂点を使用し、saturn という変数でその頂点を定義し、saturn が father であるというラベルの付いたエッジが格納されている頂点を検索します。 `in("father")` メソッドによってそのような着信エッジを選択し、`values("name")` によってその頂点の name プロパティーの値を返します。

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); ;g.V(saturn).in(\"father\").values(\"name\")"}'
```

この照会を実行すると、グラフに含まれている saturn の子の名前が返されます (この場合は、jupiter という名前が 1 つだけ返されます)。

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

2 つ目の `in("father")` 操作を組み込むと、トラバーサルによって saturn という頂点の孫が抽出されます。

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); ;g.V(saturn).in(\"father\").in(\"father\").values(\"name\")"}'
```

この照会を実行すると、saturn の孫の名前が hercules だということがわかります。

## 8. グラフをトラバースします (場所)。

GraphOfTheGods のいくつかのエッジには、緯度と経度をから成る place プロパティーがあります。 このプロパティーを使用すれば、出来事の場所を確認できます。 battled というラベルの付いたエッジには、戦いが起きた場所を示す place プロパティーがあります。 例えば、アテネ (緯度: 37.97、経度: 23.72) から半径 50 キロ以内の場所で起きたすべての戦いを抽出する場合は、そうした座標で半径を指定し、place プロパティーの値がその範囲になっているエッジをグラフ内で検索します。

`has("place", geoWithin(Geoshape.circle(37.97, 23.72, 50)))` を使用して、そのようなエッジを選択できます。

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); g.E().has(\"place\", geoWithin(Geoshape.circle(37.97, 23.72, 50)))"}'
```

この照会を実行すると、着信頂点と発信頂点のエッジのラベルと ID が返されます。 もっと読みやすい応答を出力し、どの神々がその 2 つの戦いで戦ったのかを確認するには、`as()` を使用して、返される値を _god1_ と _god2_ としてスタッシュしてから、`select()` を使用してそれらの値を取り込み、`by("name")` を使用してそれぞれの戦いを戦った神々の名前を抽出します。

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

## 9. グラフをトラバースします (頂点)。

前の手順では、2 つの `in()` エレメントを使用して、hercules が saturn の孫であることを確認しました。 Gremlin では述部の反復のために `repeat` を使用できるので、そのトラバーサルをループとして記述することもできます。

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); def hercules=g.V(saturn).repeat(__.in(\"father\")).times(2).next()"}'
```

hercules という頂点を開始点として、さらに高度な操作を実行できます。 例えば、以下のようなことが可能です。

- father と mother というラベルの付いたエッジをトラバースして、hercules の親の type を確認できます。

  ```
  curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); def hercules=g.V(saturn).repeat(__.in(\"father\")).times(2).next(); g.V(hercules).out(\"father\", \"mother\").label();" }'
  ```

- battled というエッジをトラバースして、hercules と戦った神々を確認できます。

  ```
  curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); def hercules=g.V(saturn).repeat(__.in(\"father\")).times(2).next(); g.V(hercules).out(\"battled\").valueMap()" }' 
  ```

- battled というエッジをトラバースして、hercules と複数回戦った神々を確認できます。

  ```
  curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); def hercules=g.V(saturn).repeat(__.in(\"father\")).times(2).next(); g.V(hercules).outE(\"battled\").has(\"time\", gt(1)).inV().values(\"name\")" }' 
  ```
  
