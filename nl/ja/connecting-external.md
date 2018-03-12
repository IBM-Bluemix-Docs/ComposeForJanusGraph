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

# JanusGraph への接続

JanusGraph は、HTTP 要求または WebSocket を使用した接続をサポートします。 すべての接続は HTTPS を介して行われ、認証を必要とします。 HTTP ベースの要求は、基本認証とトークン認証をサポートします。 JanusGraph デプロイメントにおいては、1 回限りの呼び出し以外では、WebSocket またはトークン認証を使用することがパフォーマンスの向上につながります。 基本認証は、要求をスローダウンさせる負荷の高いプロセスです。

HTTPS の使用法、およびグラフの作成とオープンについては、[HTTPS を使用したグラフの作成とトラバース](./tutorial-https.html)を参照してください。
{: .tip}

クライアントとデータベース間の実際の通信には、Gremlin で要求を送信するクライアント、グラフ構造の照会向けにカスタマイズされた Groovy 言語のダイアレクト、照会に応答するデータベースが関係します。 Gremlin Console を使用して要求を送信することもできます。

Gremlin Console の入手、インストール、構成、使用する方法について詳しくは、[Gremlin Console を使用したグラフの作成とトラバース](./tutorial-gremlin-console.html)を参照してください。
{: .tip}

## HTTP 要求

HTTP 要求を使用してサーバーに接続する場合は、単一エンドポイントを使用して JanusGraph とやり取りできます。単一エンドポイントは、デプロイメント内のすべての使用可能ノードを対象にラウンドロビンを行うようにセットアップされます。 これには、ホスト名とポートが含まれます。

HTTPS 接続ストリングはサービス・ダッシュボードの概要ページで分かります。 接続ストリングは、サーバーに接続するための管理者ユーザー資格情報を使用する基本認証を必要とします。

HTTP 要求を使用して {{site.data.keyword.composeForJanusGraph}} に接続するには、エンドポイントを指定した HTTP POST メソッドを使用し、「gremlin」をキー、送信する Gremlin 照会を値とする単一エントリーから成る JSON フォーマットの文書を送信する必要があります。 

```json
{
    "gremlin": "a gremlin query would go here"
}
```

Gremlin は広範囲トラバーサル言語ですが、一方そのプログラムは「1+1」のようにシンプルにすることができます。 それを `curl` コマンドでスクリプトとして渡す場合、POST 要求のデータ部は次のようになります。

```
'{"gremlin" : "1+1" }'
``` 

同じスクリプトをサーバーに数回送信する場合は、gremlin スクリプトで使用できる変数のマップを含む「bindings」オプション・ディクショナリーを準備することで、パフォーマンスを上げることができます。 これがパフォーマンスに役立つのは、サーバーが gremlin スクリプトを再コンパイルする必要がなくなるからです。
{: .tip}

HTTP プロトコルに関する追加情報が、Apache Tinkerpop Documentation の『[Connecting via REST](http://tinkerpop.apache.org/docs/3.2.3/reference/#_connecting_via_rest)』セクションに記載されています。

Gremlin スクリプトをエンドポイントに送信するには、認証される必要があります。 HTTP 要求アクセスで認証されるには、基本認証とトークン認証の 2 とおりの方法があります。

## 基本認証

基本認証では、要求と一緒にユーザー名とパスワードが送信されます。 1 回限りの照会の場合は合理的ですが、それを超える回数になると、パスワードの交換や検証を行うことの複雑さがサーバーの負荷になり始めます。 curl では、次のように `-u` オプションを指定してユーザー名とパスワードを渡すことができます。

```shell
$ curl -XPOST -d '{"gremlin" : "1+1" }' "https://portal91-0.ianuspater.compose-3.composedb.com:17916" -u admin:PASSWORDHERE
```

エンドポイント URL にユーザー名とパスワードを組み込むこともできます。 

## トークン認証

HTTP 要求の効率を上げるため、他方のエンドポイントからセッション・トークンを取得できます。 後続のすべての要求のヘッダー内でこのトークンを渡します。 HTTP エンドポイントで複数回の呼び出しを行う場合は、トークン認証を使用するとパフォーマンスが高まります。

サービス・ダッシュボードの概要ページの_「セッション (Session)」_パネルに表示される接続ストリングのいずれかを使用します。 接続ストリングには、トークンを取得するために必要な呼び出しが curl コマンドとして既に含まれています。

```shell
$ curl -XGET "https://admin:PASSWORDHERE@portal91-0.ianuspater.compose-3.composedb.com:17916/session"
```

返される JSON 結果には、セッション・トークンが含まれています。

```
{
  "token": "YWRtaW46MTQ5NjkyODM1NDgzMzpWYXlxMERsMWs0VmltdGo3WUpzMEwtN3prUi00TGxZR3J6LXZnbDVmN3lnPQ=="
}
```

トークンは 60 分間有効です。
{: .tip}

トークン値を抽出し、それを以降の要求の `Authorization` ヘッダーとして組み込みます。

```shell
$ curl -XPOST -H "Authorization: Token YWRtaW46MTQ5NjkyODM1NDgzMzpWYXlxMERsMWs0VmltdGo3WUpzMEwtN3prUi00TGxZR3J6LXZnbDVmN3lnPQ==" -d '{"gremlin" : "1+1" }' "https://portal91-0.ianuspater.compose-3.composedb.com:17916"
```

この操作をシェルから行い、[`jq`](https://stedolan.github.io/jq/) コマンドが使用可能な場合は、コマンドを実行する環境変数を次のように設定できます。

```shell
$ export JGTOKEN=`curl -s -XGET "https://admin:PASSWORDHERE@portal91-0.ianuspater.compose-3.composedb.com:17916/session" | jq -r .token`
```

その後、次のように要求を実行します。

```shell
$ curl -XPOST -H "Authorization: Token $JGTOKEN" -d '{"gremlin" : "1+1" }' "https://portal91-0.ianuspater.compose-3.composedb.com:17916"
```

## HTTP JSON 応答

返される照会への応答は JSON 文書の形式で、この文書には要求 ID 値、状況オブジェクト、結果オブジェクトが含まれます。 以下に応答の例を示します。

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
`requestId` は、要求の固有 ID です。 `status` には、可読メッセージ、HTTP 状況スタイル・コード、およびレポートされる状況の他の属性が含まれます。 `result` には、データ・オブジェクト (実行された Gremlin コードの返却内容に従って構造化されたもの) と、結果データの有効範囲外の補足情報があればそのメタ・セクションが含まれます。

## Websocket

WebSocket は、TCP と TLS を介した長寿命両方向接続を作成する効率的な手段であり、クライアント・アプリケーションと JanusGraph サーバーの間の対話式セッションに最も適しています。 JanusGraph 用のいくつかのライブラリーは、サーバーへの接続として WebSocket を使用します。Compose 上の JanusGraph と連動するには、ライブラリーは基本認証を行えて、WSS (TLS を介したセキュア WebSocket) を使用できなければなりません。 

Websocket 接続ストリングはサービス・ダッシュボードの概要ページの_「Websocket」_セクションで分かります。 URI を使用して、JanusGraph デプロイメントとの長期実行セッションを確立できます。接続がセキュア HTTPS になることを示すために、URI の接頭部として `wss:` が付けられます。 これらの接続ストリングでは、サーバーに接続するための管理者ユーザー資格情報とともに基本認証を使用する必要があります。

## Gremlin Console

Gremlin Console は、Tinkerpop 対応サーバーに関する作業をするための必須ツールです。 Gremlin Console は WebSocket 接続を使用しますが、接続に URI 構文を使用しません。一つには、接続を扱うためには他の要素も構成する必要があるからです。

Gremlin Console を構成するには、適切な詳細を含んだ YAML ファイルを準備します。 この構成は、サービス・ダッシュボードの概要ページの_「Gremlin Console YAML」_パネルに表示されます。

Gremlin Console の入手、インストール、構成、使用する方法について詳しくは、[Gremlin Console を使用したグラフの作成とトラバース](./tutorial-gremlin-console.html)を参照してください。
