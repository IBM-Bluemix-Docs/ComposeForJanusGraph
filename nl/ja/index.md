---

Copyright:
  Years: 2017
lastupdated: "2017-10-24"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Compose for JanusGraph の概説
{: #getting-started-with-compose-for-janusgraph}

{{site.data.keyword.composeForJanusGraph_full}} は、相互の関連性の高いデータを格納したり照会したりする処理に最適化したスケーラブルなグラフ・データベースです。このデータベースでは、データを多数の頂点とエッジとしてモデル化します。JanusGraph と Apache Tinkerpop(TM) との間の互換性によって、そうした複雑な構造からシンプルな方法で効率的にデータを取得することが可能になり、従来型のリレーショナル・データベースでは難しかったり不可能だったりする照会でも、効率的に実行できます。{{site.data.keyword.composeForJanusGraph}} で JanusGraph を管理すれば、JanusGraph の威力がさらに高まります。この使いやすいスケーラブルなデプロイメント・システムには、高可用性、冗長性、ノンストップの自動バックアップなどの各種機能が用意されています。
{:shortdesc}

## Compose for JanusGraph サービス・インスタンスの作成

[{{site.data.keyword.composeForJanusGraph}} インスタンスを作成します](https://console.bluemix.net/catalog/services/compose-for-janusgraph/)。

サービスのインスタンスを作成するときは、必ずサービスの名前と資格情報名の両方を選択してください。サービスをバインドしないでおきます。サービスをプロビジョンするときに指定される資格情報を使用して、アプリケーションをサービスに接続できます。各種の資格情報の値とアプリケーションでその値を使用する方法の詳細については、[{{site.data.keyword.cloud}} アプリケーションの接続](./connecting-bluemix-app.html)を参照してください。

サービス・インスタンスのプロビジョンでは、*標準*プランと*エンタープライズ*・プランのどちらかを選択できます。*エンタープライズ*・プランの場合は、インスタンスを有効な {{site.data.keyword.composeEnterprise}} クラスターにプロビジョンできます。{{site.data.keyword.composeEnterprise}} は、企業コンプライアンスで要求されるセキュリティーと分離を提供し、専用ネットワーキングを使用してデプロイ済みデータベースのパフォーマンスを確保します。詳しくは、[Compose Enterprise 文書](../ComposeEnterprise/index.html)を参照してください。

## Compose for JanusGraph の管理

サービス・ダッシュボードからサービスを管理できます。ダッシュボードには {{site.data.keyword.cloud_notm}} Compose データベースとそれへの接続方法に関する情報が示されます。また、以下の操作を行うこともできます。
- バックアップを管理する
- サービスに割り振るリソースを増やす
- サービス・パスワードを変更する
- ホワイトリストを使用してデータベースへのアクセスを制限する
詳しくは、[設定](./dashboard-settings.html)を参照してください。

## Compose for JanusGraph への接続

サービスに接続するには、サービスと一緒に作成された資格情報を使用するか、サービス・ダッシュボードの*「概要」*タブに表示される接続ストリングとコマンド・ラインを使用します。

{{site.data.keyword.cloud_notm}} の外部からサービスに接続する場合は、用意されている接続ストリングやコマンド・ラインを使用できます。詳しくは、[JanusGraph への接続](./connecting-external.html)を参照してください。

## {{site.data.keyword.cloud_notm}} アプリケーションの接続

{{site.data.keyword.cloud_notm}} アプリケーションをサービスに接続するには、サービスと一緒に作成された資格情報を使用します。{{site.data.keyword.cloud_notm}} アプリケーションを {{site.data.keyword.composeForJanusGraph}} サービスに接続する方法については、[{{site.data.keyword.cloud_notm}} アプリケーションの接続](./connecting-bluemix-app.html)を参照してください。

## JanusGraph データ・ブラウザーの使用

コマンド・ラインからグラフ・データを探索するのは、複雑な作業になることがあります。{{site.data.keyword.composeForJanusGraph}} のデータ・ブラウザーでは、使いやすい照会ビルダーと高機能の照会応答カードを組み合わせて、照会結果を対話式の JSON ビューの形でもビジュアルなグラフの形でも表示できます。詳しくは、[JanusGraph データ・ブラウザーの使用](./data-browser.html)を参照してください。
