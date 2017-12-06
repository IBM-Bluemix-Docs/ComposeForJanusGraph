---

Copyright:
  Years: 2017
lastupdated: "2017-09-05"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# サービス概要

{{site.data.keyword.cloud}} Compose データベースに関する情報は_「概要」_ページに表示されます。概要には、必要不可欠な識別情報と現在のリソース使用率が含まれます。ツールで使用する接続ストリングのセクションもあります。接続ストリングを使用してツールからデータベースに接続することもできます。

## デプロイメントの詳細 (Deployment Details)

_「デプロイメントの詳細 (Deployment Details)」_パネルには、サービスの詳細が表示されます。

![「デプロイメントの詳細 (Deployment Details)」](./images/janusgraph-deployment-details.png "「デプロイメントの詳細 (Deployment Details)」パネルのビュー")

### タイプ

サービスによって提供されるデータベースのタイプ、およびサービスが使用するデータベースのバージョン。

### 名前

サービスの内部 ID。

### 使用法

データベース・サイズとご使用のサービス・プランで利用可能なストレージ容量。


## 接続ストリング (Connection Strings)

接続ストリングはいくつかのクライアント・ライブラリーで使用でき、他のライブラリーが接続するために必要なすべての情報を含んでいます。接続ストリングを使用してサービスに接続する方法については、[外部アプリケーションの接続](./connecting-external.html)を参照してください。

_「接続ストリング」_パネルの各タブで、サービスで使用できるそれぞれの接続ストリングを確認できます。

### セッション

セッション URI を使用して、60 分間有効な許可トークンを取得できます。HTTPS または Websocket URI を使用してデプロイメントに対して呼び出しを行うときは、与えられたトークンを使用する必要があります。

### HTTPS

JanusGraph デプロイメントの基本的接続ストリングです。HTTPS 接続ストリングを使用するには、サーバーに接続するための管理者ユーザー資格情報を指定する必要があります。HTTPS の使用法について詳しくは、[HTTPS を使用したグラフの作成とトラバース](./tutorial-https.html)を参照してください。

### Websocket

Websocket URI を使用して、JanusGraph デプロイメントとの長期実行セッションを確立できます。接続がセキュア HTTPS になることを示すために、接頭部として `wss:` が付けられます。これらの接続ストリングでは、サーバーに接続するための管理者ユーザー資格情報とともに基本認証を使用する必要があります。

JanusGraph 用のいくつかのライブラリーは、サーバーへの接続として WebSocket を使用します。{{site.data.keyword.composeForJanusGraph}} と連動するには、ライブラリーは基本認証を行えて、WSS (TLS を介したセキュア WebSocket) を使用できなければなりません。

### Gremlin Console YAML

示された構成のいずれかを Gremlin Console で使用して、{{site.data.keyword.composeForJanusGraph}} に接続できます。Gremlin Console YAML の使用法について詳しくは、[Gremlin Console を使用したグラフの作成とトラバース](./tutorial-gremlin-console.html)を参照してください。

