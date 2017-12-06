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

# Gremlin Console を使用したグラフの作成とトラバース

このトピックでは、{{site.data.keyword.composeForJanusGraph_full}} データベースを初めて使用する時の手順を取り上げます。このチュートリアルでは、Gremlin Console を使用して JanusGraph サーバーにコマンドを送信します。

## 1. Gremlin をインストールします。

Gremlin Console をインストールするには、以下のようにします。

1. 最新バージョンの Java がインストールされていることを確認します。
2. [Gremlin Console バージョン 3.2.3](https://archive.apache.org/dist/tinkerpop/3.2.3/apache-tinkerpop-gremlin-console-3.2.3-bin.zip) をダウンロードします。
3. ダウンロードしたファイルを作業ディレクトリーに unzip します。
4. コマンド・ラインか端末を使用して Gremlin Console のルート・ディレクトリーに移動し、`bin/gremlin.sh` を実行してダウンロードの内容を確認します。

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

5. Gremlin Console を構成します。{{site.data.keyword.composeForJanusGraph}} サービスの*「概要」*ページに、Gremlin Console の YAML があります。いずれかの構成を `conf` ディレクトリー内のファイルに保存します。このサンプルでは、`conf/compose.yaml` として保存します。
 
## 2. Compose for JanusGraph に接続します。

接続を確認するために、`bin/gremlin.sh` を再び実行します。`:remote` コマンドを使用して tinkerpop サーバーに接続します。

```text
:remote connect tinkerpop.server conf/compose.yaml session
```

ここでは、Gremlin に対して `connect` コマンドを送信しています。`conf/compose.yaml` の構成に基づいて `tinkerpop.server` に接続するためのコマンドです。さらに、`session` という引数を渡して、セッション・サポートを有効にしています。

`:help :remote` と入力すると、`:remote` コマンドのオプションが表示されます。
{: .tip}

接続に成功すると、SSL に関する警告と接続の確認メッセージが表示されます。

```text
WARN  org.apache.tinkerpop.gremlin.driver.Cluster  - SSL configured without a trustCertChainFile and thus trusts all certificates without verification (not suitable for production)
==>Configured portal93-4.ianuspater.compose-3.composedb.com/159.8.153.216:18045-[2378d0de-0a93-4330-b8d8-848667f5b117]
gremlin>
```

## 3. サーバーにコマンドを送信します。

Gremlin Console は、何でも自動的にリモート・サーバーに転送するわけではありません。サーバーにコマンドを送信するには、コマンドに `:>` という接頭部を付ける必要があります。あるいは、`:remote console` によってリモート・サーバーにすべてのコンソール・コマンドをリダイレクトすることもできます。その点を示した以下の一連のコマンドと応答をご覧ください。

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

## 4. グラフを作成します。

{{site.data.keyword.composeForJanusGraph}} には、グラフを作成したり開いたり閉じたりするための専用のグラフ・ファクトリーがあります。このファクトリー (`ConfiguredGraphFactory`) を使用すれば、基礎になっているストレージ・メカニズムについて知る必要はありません。グラフに名前を付けるだけで、新しいグラフを作成できます。最初に、新しいグラフを作成してください。このチュートリアルのサンプル・コマンドでは、グラフ名を _mygraph_ にしています。

`def` キーワードを使用してグラフの変数を宣言します。

```
def graph=ConfiguredGraphFactory.create("mygraph")
```

{{site.data.keyword.composeForJanusGraph}} のグラフ名には、英数字と下線文字しか使用できません。
{: .tip}

## 5. グラフを開きます。

グラフを作成したら、グラフを操作するためにまず開く必要があります。そのために、`ConfiguredGraphFactory` で `open()` を呼び出します。それでは、次のようにグラフを開いてください。

```
def graph=ConfiguredGraphFactory.open("mygraph")
```

この時点で、永続的なグラフにするために操作をコミットする必要があります。

```
graph.tx().commit()
```

この時点でセッションを閉じても、再びセッションを開始すると、この新しいグラフが表示されます。

## 6. グラフに頂点を追加します。

このグラフに、ソフトウェア開発者の情報とその開発者たちが作成したソフトウェアの情報を組み込みます。ここでは、marko という開発者と lop という名前のソフトウェアを追加します。

まず、marko を追加します。

```
def v1 = graph.addVertex(T.label, "person", "name", "marko", "age", 29)
```

成功すると、追加した頂点の索引 (`==>v[4304]`) が返されます。次に、2 つ目の頂点を追加します。

```
def v2 = graph.addVertex(T.label, "software", "name", "lop", "lang", "java")
```

ここでも、操作をコミットする必要があります。

```
graph.tx().commit()
```

## 7. グラフで頂点を検索します。

頂点を検索するには、まずグラフをトラバースしてから has() メソッドを使用します。そうすると、頂点の ID が返されます。ここでは、marko という名前の頂点を検索します。

```
def g=graph.traversal()
g.V().has("name","marko")
```

今までのセッションを閉じて新しいセッションを開始する時などに、これらの頂点を変数 v1 と v2 に再び割り当てる場合は、next()を使用します。

```
def v1 = g.V().has("name","marko").next()
def v2 = g.V().has("name","lop").next()
```

このようにすれば、後続の Gremlin コマンドでこれらの変数を使用できます。

## 8. エッジを追加します。

次に、2 つの頂点の間のエッジを追加し、エッジの重みを設定します。ここでは、エッジのラベルを "created"、重みを 0.4 にします。

この数値を 10 進数としてキャストする必要があるので、この値を `0.4d` として指定します。

```
v1.addEdge("created", v2, "weight", 0.4d)
```

ここでも変更内容を必ずコミットしてください。

## 9. グラフの照会を実行します。

グラフの照会を実行して、marko が作成したソフトウェアの名前を検索します。

```
g.V().has("name","marko").out("created").values("name")
```
