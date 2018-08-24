---

copyright:
  years: 2017,2018
lastupdated: "2018-05-08"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# {{site.data.keyword.cloud_notm}} 애플리케이션 연결

{{site.data.keyword.cloud}} 애플리케이션을 서비스에 연결하려면 서비스가 프로비저닝될 때 작성된 서비스 신임 정보를 사용하십시오.

## 사용 가능한 신임 정보

필드 이름|설명
----------|-----------
`deployment_id`|Compose에서 작성된 서비스의 내부 ID입니다.
`name`|데이터베이스 배치 이름입니다.
`db_type`|서비스에서 제공되는 데이터베이스의 유형입니다. 이 경우 `JanusGraph`입니다.
`uri_cli`|토큰 기반 인증으로 gremlin 조회를 전송하는 방법을 보여주는 `CURL` 명령행입니다.
`uri_cli_1`|데이터베이스 인스턴스에 연결하는 2차 `CURL` 명령행입니다.
`maps`|사용하지 않음
`uri`|서비스에 연결할 때 사용하는 `https` URI로서, 서버의 호스트 이름 및 연결할 포트 이름이 포함됩니다. [외부 애플리케이션 연결](./connecting-external.html)을 참조하십시오.
`uri_direct_1`|서비스에 연결하기 위한 2차 `https` URI입니다.
`misc`|`session`, `websocket` 및 `gremlin_console_yaml` 필드를 포함하는 상위 필드입니다.
`misc.session`|서비스와 상호작용하는 데 사용될 수 있는 60분 인증 토큰을 검색하는 데 사용하는 `https` URI입니다. [외부 애플리케이션 연결 - 토큰 인증](./connecting-external.html#token-authentication)을 참조하십시오.
`misc.websocket`|Websocket을 사용하여 서비스에 연결할 때 사용되는 `wss` URI입니다. [외부 애플리케이션 연결 - Websocket](./connecting-external.html#websockets)을 참조하십시오.
`misc.gremlin_console_yaml`|서비스에 연결하도록 Gremlin 콘솔을 구성하는 데 사용되는 YAML 정보입니다.  [외부 애플리케이션 연결 - Gremlin 콘솔](./connecting-external.html#gremlin-console)을 참조하십시오.
{: caption="표 1. Compose for JanusGraph 신임 정보" caption-side="top"}

## 신임 정보 사용

서비스 신임 정보를 사용하려면 애플리케이션이 서비스에 연결되어야 합니다. 다음 단계는 Node.js를 예제로 사용하여 이를 수행하는 방법을 보여줍니다.

### 애플리케이션을 서비스에 연결

{{site.data.keyword.cloud_notm}} 애플리케이션이 이미 있는 경우 {{site.data.keyword.cloud_notm}} 계정에서 이를 서비스에 연결할 수 있습니다.

1. {{site.data.keyword.cloud_notm}} 계정에 로그인하십시오.
2. 대시보드에서 서비스에 연결할 애플리케이션을 선택하십시오.
3. 애플리케이션 _개요_ 페이지의 연결 패널에서 **새로 연결** 또는 **기존 항목 연결**을 클릭하여 신규 또는 기존 {{site.data.keyword.cloud_notm}} 서비스에 연결하십시오.

  - **기존 항목 연결**을 클릭한 경우 사용 가능한 서비스 목록이 표시됩니다. 앱을 연결할 서비스를 선택하고 **연결**을 클릭하십시오.
  - **새로 연결**을 클릭한 경우 단계에 따라 새 서비스 인스턴스를 프로비저닝하십시오. 카탈로그에서 프로비저닝할 서비스를 선택하면 _연결 대상_ 필드가 자동으로 해당 애플리케이션의 이름으로 완료됩니다.

{{site.data.keyword.cloud_notm}}에 업로드하지 않은 애플리케이션이 있는 경우 {{site.data.keyword.cloud_notm}}에 업로드하기 전에 애플리케이션을 서비스에 바인딩할 수 있습니다. 

1. {{site.data.keyword.cloud_notm}} 계정에 로그인하십시오.
2. [Cloud Foundry CLI](https://github.com/cloudfoundry/cli) 도구를 다운로드하여 설치하십시오.
3. 명령행 도구에서 {{site.data.keyword.cloud_notm}}에 연결하고 프롬프트에 따라 로그인하십시오.

  ```
  $ cf api https://api.ng.bluemix.net
  $ cf login
  ```

4. 애플리케이션의 `manifest.yml` 파일을 여십시오.

  - `host` 값을 고유한 값으로 변경하십시오. 선택하는 호스트에 따라 애플리케이션 URL의 하위 도메인이 결정됩니다(`<host>.mybluemix.net`).
  - `name` 값을 변경하십시오. 선택하는 값은 {{site.data.keyword.cloud_notm}} 대시보드에 표시될 때 앱의 이름입니다.

5. `manifest.yml`의 `services` 값을 서비스의 이름과 일치하도록 업데이트하십시오. 이제 `manifest.yml`은 다음과 유사합니다.

  ```
  ---
  applications:
  - name:    compose-janusgraph-sample-service
    host:    compose-janusgraph-sample-service
    memory:  256M
    services:
      - compose-janusgraph-sample-app
  ```

### 신임 정보 액세스 및 사용

애플리케이션이 Cloud Foundry 환경 변수를 구문 분석해야 합니다. Node.js에서는 [cfenv](https://www.npmjs.com/package/cfenv) 패키지를 사용하여 이를 수행할 수 있습니다.

1. `--save`로 패키지를 설치하여 `package.json`을 업데이트하십시오.

  ```
  npm install --save cfenv
  ```

2. Cloud Foundry 환경 변수에서 {{site.data.keyword.composeForJanusGraph_full}} 신임 정보를 추출하십시오.

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

  이제 서비스 신임 정보를 사용할 수 있습니다. 예를 들어, 인증 토큰을 얻는 데 사용할 수 있는 세션 URI를 가져오기 위해 사용할 수 있습니다.

  ```javascript
  let jgurl = credentials.misc.session[0];
  ```

3. 앱을 {{site.data.keyword.cloud_notm}}에 푸시하십시오. 앱을 푸시하면 자동으로 앱이 서비스에 바인딩됩니다.

  ```
  $ cf push
  ```

애플리케이션을 {{site.data.keyword.cloud_notm}} 서비스와 함께 사용하는 방법에 대한 자세한 정보는 [앱에 서비스 추가](https://console.{DomainName}/docs/services/reqnsi.html#add_service)를 참조하십시오.
