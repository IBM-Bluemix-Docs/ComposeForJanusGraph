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

# JanusGraph에 연결

JanusGraph는 HTTP 요청을 사용하거나 WebSocket을 통한 연결을 지원합니다. 모든 연결은 HTTPS를 통해 수행되며 인증이 필요합니다. HTTP 기반 요청은 기본 및 토큰 인증을 지원합니다. JanusGraph 배치에 대한 일회성 호출 이외의 모든 경우에 WebSocket 또는 토큰 인증을 사용하면 성능이 향상됩니다. 기본 인증은 요청 속도를 느리게 하는 높은 비용의 프로세스입니다.

HTTP를 사용하는 방법과 그래프 작성 및 열기에 대해 자세히 알아보려면 [HTTPS를 사용하여 그래프 작성 및 순회](./tutorial-https.html)를 참조하십시오.
{: .tip}

클라이언트와 데이터베이스 간의 실제 통신에는 Gremlin에서 요청을 전송하는 클라이언트, 그래프 구조를 조회하기 위해 사용자 정의된 Groovy 언어의 언어 및 해당 조회에 응답하는 데이터베이스가 포함됩니다. Gremlin 콘솔을 사용하여 요청을 전송할 수도 있습니다.

Gremlin 콘솔을 가져오고 설치, 구성 및 사용하는 방법에 대한 세부사항은 [Gremlin 콘솔을 사용하여 그래프 작성 및 순회](./tutorial-gremlin-console.html)를 참조하십시오.
{: .tip}

## HTTP 요청

HTTP 요청을 사용하여 서버에 연결하는 경우 단일 엔드포린트를 사용하여 JanusGraph와 통신할 수 있으며 배치에 있는 사용 가능한 모든 노드에서 라운드 로빈으로 설정됩니다. 여기에는 호스트 이름과 포트가 포함됩니다.

서비스 대시보드 개요 페이지에서 HTTPS 연결 문자열을 가져올 수 있습니다. 연결 문자열을 사용하려면 관리자 신임 정보를 사용하여 서버에 연결하는 기본 인증이 필요합니다. 

HTTP 요청을 사용하여 {{site.data.keyword.composeForJanusGraph}}에 연결하려면 엔드포인트에 HTTP POST 메소드를 사용하고 "gremlin" 키와 전송하려는 Gremlin 조회의 값이 포함된 단일 항목과 함께 JSON 형식의 문서를 전송해야 합니다.  

```json
{
    "gremlin": "a gremlin query would go here"
}
```

Gremlin은 광범위한 순회 언어이지만 Gremlin의 프로그램은 "1+1"만큼 단순할 수도 있습니다. 이를 `curl` 명령의 스크립트로 전달하려는 경우 POST 요청의 데이터 파트는 다음과 같습니다.

```
'{"gremlin" : "1+1" }'
``` 

동일한 스크립트를 서버에 여러 번 전송하는 경우 gremlin 스크립트에 사용 가능한 변수의 맵이 포함된 선택적 "bindings" 사전을 제공하여 성능을 향상시킬 수 있습니다. 서버가 gremlin 스크립트를 다시 컴파일하지 않아도 되기 때문에 성능에 도움이 됩니다.
{: .tip}

HTTP 프로토콜에 대한 추가 정보는 Apache Tinkerpop 문서의 [REST를 통해 연결](http://tinkerpop.apache.org/docs/3.2.3/reference/#_connecting_via_rest) 섹션에서 찾을 수 있습니다.

Gremlin 스크립트를 엔드포인트에 전송하려면 인증되어야 합니다. HTTP 요청 액세스에 인증되는 두 가지 방법(기본 인증 및 토큰 인증)이 있습니다.

## 기본 인증

기본 인증은 사용자 이름 및 비밀번호를 요청과 함께 전송합니다. 일회성 조회의 경우에는 적절하지만 그 이상이 되면 서버에 비밀번호 교환 및 유효성 검증을 수행하는 복잡도를 부과하기 시작합니다. curl을 사용하면 `-u` 옵션으로 사용자 및 비밀번호를 전달할 수 있습니다.

```shell
$ curl -XPOST -d '{"gremlin" : "1+1" }' "https://portal91-0.ianuspater.compose-3.composedb.com:17916" -u admin:PASSWORDHERE
```

사용자 이름과 비밀번호를 엔드포인트 URL에 임베드할 수도 있습니다. 

## 토큰 인증

HTTP 요청의 효율성을 높이기 위해 다른 엔드포인트에서 세션 토큰을 얻을 수 있습니다. 이 토큰은 모든 후속 요청의 헤더에 전달됩니다. HTTP 엔드포인트에 대해 일회성 호출 이외의 모든 작업을 수행하는 경우 토큰 인증을 사용하여 성능을 향상시킬 수 있습니다.

서비스 대시보드 개요 페이지의 _세션_ 패널에 있는 연결 문자열 중 하나를 사용하십시오. 연결 문자열에는 curl 명령으로 토큰을 얻는 데 필요한 호출이 이미 포함되어 있습니다.

```shell
$ curl -XGET "https://admin:PASSWORDHERE@portal91-0.ianuspater.compose-3.composedb.com:17916/session"
```

리턴된 JSON 결과에 세션 토큰이 있습니다.

```
{
  "token": "YWRtaW46MTQ5NjkyODM1NDgzMzpWYXlxMERsMWs0VmltdGo3WUpzMEwtN3prUi00TGxZR3J6LXZnbDVmN3lnPQ=="
}
```

토큰은 60분 동안 유효합니다.
{: .tip}

토큰 값을 추출한 후 향후 요청에 `Authorization` 헤더로 포함시키십시오.

```shell
$ curl -XPOST -H "Authorization: Token YWRtaW46MTQ5NjkyODM1NDgzMzpWYXlxMERsMWs0VmltdGo3WUpzMEwtN3prUi00TGxZR3J6LXZnbDVmN3lnPQ==" -d '{"gremlin" : "1+1" }' "https://portal91-0.ianuspater.compose-3.composedb.com:17916"
```

쉘에서 이를 수행 중이고 [`jq`](https://stedolan.github.io/jq/) 명령이 사용 가능한 경우 다음과 같은 명령을 실행하여 환경 변수를 설정할 수 있습니다.

```shell
$ export JGTOKEN=`curl -s -XGET "https://admin:PASSWORDHERE@portal91-0.ianuspater.compose-3.composedb.com:17916/session" | jq -r .token`
```

그런 다음, 다음과 같은 요청을 실행하십시오.

```shell
$ curl -XPOST -H "Authorization: Token $JGTOKEN" -d '{"gremlin" : "1+1" }' "https://portal91-0.ianuspater.compose-3.composedb.com:17916"
```

## HTTP JSON 응답

뒤따르는 조회에 대한 응답은 요청 ID 값, 상태 오브젝트 및 결과 오브젝트가 포함된 JSON 문서의 양식으로 되어 있습니다. 다음은 응답의 예입니다.

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
`requestId`는 요청에 대한 고유 ID입니다. `status`에는 읽을 수 있는 메시지, HTTP 상태 스타일 코드 및 보고된 상태의 기타 속성이 포함됩니다. `result`에는 실행된 Gremlin 코드가 리턴하는 내용에 따라 구조화되는 데이터 오브젝트와 결과 데이터의 범위 외부에 있는 추가 정보에 대한 메타 섹션이 포함됩니다.
## Websocket

WebSocket은 클라이언트 애플리케이션과 JanusGraph 서버 간의 대화식 세션에 적합한 TCP 및 TLS를 통해 수명이 긴 양방향 연결을 작성하는 효율적인 방법입니다. 다수의 JanusGraph용 라이브러리에서 WebSocket을 서버에 대한 연결로 사용합니다. JanusGraph on Compose에 대해 작업하려면 기본 인증을 수행하고 WSS(TLS를 통한 보안 WebSockets)를 사용할 수 있어야 합니다. 

서비스 대시보드 개요 페이지의 _Websocket_ 섹션에서 Websocket 연결 문자열을 가져올 수 있습니다. URI를 사용하여 JanusGraph 배치와의 장기 실행 세션을 설정할 수 있으며 연결이 HTTPS로 보안됨을 표시하기 위해 URI에 `wss:`라는 접두부가 지정됩니다. 이러한 연결 문자열을 사용하려면 관리자 신임 정보로 서버에 연결하는 기본 인증이 필요합니다.

## Gremlin 콘솔

Gremlin 콘솔은 Tinkerpop 사용 서버에 대해 작업하기 위한 필수 도구입니다. Gremlin 콘솔은 WebSockets 유틸리티를 사용하지만 연결을 위해 URI 구문을 사용하지 않습니다. 이는 부분적으로 연결에 대해 작업하려면 다른 요소도 구성해야 하기 때문입니다.

Gremlin 콘솔을 구성하기 위해 해당 세부사항이 포함된 YAML 파일을 제공합니다. 서비스 대시보드 개요 페이지의 _Gremlin 콘솔 YAML_ 패널에서 이 구성을 찾을 수 있습니다.

Gremlin 콘솔을 가져오고 설치, 구성 및 사용하는 방법에 대한 세부사항은 [Gremlin 콘솔을 사용하여 그래프 작성 및 순회](./tutorial-gremlin-console.html)를 참조하십시오.
