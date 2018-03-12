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

# JanusGraph ホスト・サービスに関する注意点

{{site.data.keyword.composeForJanusGraph_full}} はホスト・サービスなので、デプロイメントを保護する方法は、専用サーバーで実行する JanusGraph の場合とは異なる場合があります。このトピックでは、そうした違いを取り上げます。これらの変更のほとんどは Gremlin 言語の照会の動作に影響を与えます。

## サンドボックス

Gremlin 照会で、システムにダメージを与えるような機能にアクセスすることがないようにするために、すべての照会はサンドボックスで実行されます。したがって、すべての関数呼び出しとメソッド呼び出しで、以下のいずれかの正規表現に一致するシグニチャーが必要になります。

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

関数のホワイトリストに含まれている項目に一致しない関数呼び出しやメソッド呼び出しを実行しようとすると、エラーになります。 

```
[Static type checking] - Not authorized to call this method: ...
```

また、サンドボックスでは、安全検査を実施するために、すべての変数を静的に宣言する必要があります。この操作は、`def` コマンドで実行することができます。

## セッション

セッションを使用すれば、{{site.data.keyword.composeForJanusGraph}} への接続時に、要求と要求の間で状態を保持できます。{{site.data.keyword.composeForJanusGraph}} に対する接続のオプションのほとんどには、セッションが関連付けられていません。したがって、そうした接続方式で送信する Gremlin スクリプトは、完全に自己完結型にしなければなりません。例えば、スクリプトで処理する必要のあるグラフを開き、適切なノードを照会し、そのようなノードからグラフをトラバースする場合は、そうした操作をすべて 1 つの要求で処理する必要があります。この制限は、HTTP 要求と WebSocket 接続の両方に当てはまります。 

ただし、Gremlin コンソールから `:remote connect` を使用して接続する場合は例外です。

```
:remote connect tinkerpop.server conf/compose.yaml
```

上記のコマンドを使用して接続する場合はセッションがなく、接続の動作は HTTP 要求の場合と同じになります。しかし、`session` を引数として追加すれば、セッション・サポートが有効になります。

```
:remote connect tinkerpop.server conf/compose.yaml session
```

セッションを使用すると、1 つのコマンドで変数を定義して別のコマンドでその変数を参照することが可能になります。
{: .tip}

## トランザクション

基礎になっている JanusGraph データベースへのすべての変更は、1 つのトランザクションとしてカプセル化されます。トランザクションを使用する場合は、トランザクション内のすべての変更をコミットして永続的な変更にすることも、コミットしないようにするためにロールバックすることもできます。 

HTTP 要求や WebSocket で要求を実行する場合は、データベースへの書き込みが発生する変更を行うと、トランザクションが自動的に開始されます。またそのトランザクションは、要求の完了時に自動的にコミットされます。

ただし、Gremlin コンソールからセッションを有効にして接続する場合は例外です。セッションは長時間続く可能性があるので、ユーザーは任意で `graph.tx().commit()`  (`graph` は、変更が含まれている開かれたグラフ) を呼び出して変更をコミットすることもできます。また、`graph.tx().rollback()` を呼び出して、トランザクション開始前のグラフの状態に戻すこともできます。 

## 混合索引

現行バージョンの {{site.data.keyword.composeForJanusGraph}} は、混合索引をサポートしていません。
