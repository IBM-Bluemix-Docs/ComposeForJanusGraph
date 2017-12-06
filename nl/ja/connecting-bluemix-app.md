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

# {{site.data.keyword.cloud_notm}} アプリケーションの接続

{{site.data.keyword.cloud}} アプリケーションをサービスに接続するには、サービスがプロビジョンされた時に作成されたサービス資格情報を使用します。

## 使用可能な資格情報

フィールド名|説明

----------|-----------
`deployment_id`|Compose 内で作成された、サービスの内部 ID。

`name`|データベース・デプロイメント名。

`db_type`|サービスによって提供されるデータベースのタイプ。この場合は、`JanusGraph`。

`uri_cli`|トークン・ベースの認証で gremlin 照会を送信する方法を示す `CURL` コマンド・ライン。
`uri_cli_1`|データベース・インスタンスに接続する代替 `CURL` コマンド・ライン。

`maps`|未使用
`uri`|サービスに接続するときに使用する `https` URI。接続先のサーバーのホスト名とポート番号が含まれます。
[外部アプリケーションの接続](./connecting-external.html)を参照してください。
`uri_direct_1`|サービスに接続するための代替 `https` URI。
`misc`|`session`、`websocket`、および `gremlin_console_yaml` フィールドを含む親フィールド。
`misc.session`| サービスとの対話に使用できる 60 分認証トークンを取得するために使用する `https` URI。[外部アプリケーションの接続 - トークン認証](./connecting-external.html#token-authentication)を参照してください。
`misc.websocket`|Websocket を使用してサービスに接続するときに使用する `wss` URI。[外部アプリケーションの接続 - Websocket](./connecting-external.html#websockets) を参照してください。
`misc.gremlin_console_yaml`|サービスに接続するように Gremlin コンソールを構成するために使用する YAML 情報。[外部アプリケーションの接続 - Gremlin コンソール](./connecting-external.html#gremlin-console)を参照してください。
{: caption="表 1. Compose for JanusGraph の資格情報" caption-side="top"}

## 資格情報の使用

サービス資格情報を使用するには、アプリケーションをサービスに接続する必要があります。以下のステップは、その方法を示しています (例として Node.js を使用)。

### サービスへのアプリケーションの接続

既に {{site.data.keyword.cloud_notm}} アプリケーションがある場合は、ご使用の {{site.data.keyword.cloud_notm}} アカウントからサービスに接続できます。

1. {{site.data.keyword.cloud_notm}} アカウントにログインします。
2. ダッシュボードから、サービスに接続するアプリケーションを選択します。
3. アプリケーション_「概要」_ページの接続パネルで、**「新規に接続」**または**「既存に接続」**をクリックして、新規または既存の {{site.data.keyword.cloud_notm}} サービスに接続します。

  - **「既存に接続」**をクリックした場合は、選択可能サービスのリストが表示されます。アプリケーションの接続先にするサービスを選択して、**「接続」**をクリックします。
  - **「新規に接続」**をクリックした場合は、新しいサービス・インスタンスをプロビジョンするためのステップに従ってください。プロビジョンするサービスをカタログから選択すると、_「接続」_フィールドにアプリケーションの名前が自動的に入力されます。

{{site.data.keyword.cloud_notm}} にアップロードされていないアプリケーションがある場合は、アプリケーションをサービスにバインドしてから {{site.data.keyword.cloud_notm}} にアップロードしてください。 

1. {{site.data.keyword.cloud_notm}} アカウントにログインします。
2. [Cloud Foundry CLI](https://github.com/cloudfoundry/cli) ツールをダウンロードしてインストールします。
3. コマンド・ライン・ツールで {{site.data.keyword.cloud_notm}} に接続し、プロンプトに従ってログインします。

  ```
  $ cf api https://api.ng.bluemix.net
  $ cf login
  ```

4. アプリケーションの `manifest.yml` ファイルを開きます。

  - `host` 値を固有のものに変更します。アプリケーションの URL のサブドメインは、`<host>.mybluemix.net` のように、選択したホストによって決まります。
  - `name` 値を変更します。選択した値は、{{site.data.keyword.cloud_notm}} ダッシュボードでのアプリケーションの表示名になります。

5. `manifest.yml` 内の `services` 値を更新して、サービスの名前と一致させます。この段階で、`manifest.yml` は次のようになります。

  ```
  ---
  applications:
  - name:    compose-janusgraph-sample-service
    host:    compose-janusgraph-sample-service
    memory:  256M
    services:
      - compose-janusgraph-sample-app
  ```

### 資格情報へのアクセスと使用

アプリケーションは Cloud Foundry 環境変数を解析する必要があります。Node.js では、[cfenv](https://www.npmjs.com/package/cfenv) パッケージを使用して行えます。

1. `--save` を指定してパッケージをインストールして `package.json` を更新します。

  ```
  npm install --save cfenv
  ```

2. Cloud Foundry 環境変数から {{site.data.keyword.composeForJanusGraph_full}} 資格情報を抽出します。

  ```javascript
  const cfenv = require('cfenv');
  const appenv = cfenv.getAppEnv();

  // Within the application environment (appenv) there's a services object
  const services = appenv.services;

  // The services object is a map named by service so extract the one for JanusGraph
  let janusgraph_services = services["compose-for-janusgraph"];

  // We now take the first bound JanusGraph service and extract its credentials object
  let credentials = janusgraph_services[0].credentials;
  ```

  これで、サービス資格情報を使用できるようになりました。例えば、認証トークンの獲得に使用できるセッション URI を取得するには、次のようにします。

  ```javascript
  let jgurl = credentials.misc.session[0];
  ```

3. アプリケーションを {{site.data.keyword.cloud_notm}} にプッシュします。プッシュされたアプリケーションは、自動的にサービスにバインドされます。

  ```
  $ cf push
  ```

{{site.data.keyword.cloud_notm}} サービスでのアプリケーションの使用について詳しくは、[アプリケーションへのサービスの追加](https://console.bluemix.net/docs/services/reqnsi.html#add_service)を参照してください。
