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

# Gremlin 콘솔을 사용하여 그래프 작성 및 순회

이 주제는 첫 번째 {{site.data.keyword.composeForJanusGraph_full}} 데이터베이스를 시작하는 데 도움이 됩니다. 이 튜토리얼에서는 Gremlin 콘솔을 사용하여 JanusGraph 서버로 명령을 전송합니다.

## 1. Gremlin 설치

Gremlin 콘솔을 설치하려면 다음을 수행하십시오.

1. 최신 버전의 Java가 설치되어 있는지 확인하십시오.
2. [Gremlin 콘솔 버전 3.2.3](https://archive.apache.org/dist/tinkerpop/3.2.3/apache-tinkerpop-gremlin-console-3.2.3-bin.zip)을 다운로드하십시오.
3. 작업 디렉토리에 다운로드된 파일의 압축을 푸십시오.
4. 명령행 또는 터미널을 사용하여 디렉토리를 루트 Gremlin 콘솔 디렉토리로 변경하고 `bin/gremlin.sh`를 실행하여 다운로드를 확인하십시오.

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

5. Gremlin 콘솔을 구성하십시오. {{site.data.keyword.composeForJanusGraph}} 서비스의 *개요* 페이지에서 Gremlin 콘솔 YAML을 찾을 수 있습니다. 구성 중 하나를 `conf` 디렉토리의 파일에 저장하십시오. 이 예제에서는 `conf/compose.yaml`로 저장합니다.
 
## 2. Compose for JanusGraph에 연결

연결을 확인하려면 `bin/gremlin.sh`를 다시 실행하십시오. 그런 다음, `:remote` 명령을 사용하여 tinkerpop 서버에 연결하십시오.

```text
:remote connect tinkerpop.server conf/compose.yaml session
```

이 명령은 `conf/compose.yaml`의 구성을 사용하여 `tinkerpop.server`인 항목에 `connect`하도록 Gremlin에 알립니다. 또한 세션 지원을 사용하기 위해 다른 인수(`session`)를 전달합니다.

`:remote` 명령에 대한 옵션을 보려면 `:help :remote`를 입력하십시오.
{: .tip}

연결에 성공하면 SSL 경고와 연결을 확인하는 메시지가 표시됩니다.

```text
WARN  org.apache.tinkerpop.gremlin.driver.Cluster  - SSL configured without a trustCertChainFile and thus trusts all certificates without verification (not suitable for production)
==>Configured portal93-4.ianuspater.compose-3.composedb.com/159.8.153.216:18045-[2378d0de-0a93-4330-b8d8-848667f5b117]
gremlin>
```

## 3. 서버에 명령 전송

Gremlin 콘솔에서는 자동으로 원격 서버에 항목을 전달하지 않습니다. 명령을 서버에 전송하려면 명령 앞에 `:>` 접두부를 붙여야 합니다. 또는 `:remote console`을 사용하여 모든 콘솔 명령을 원격 서버로 경로 재지정할 수 있습니다. 다음 일련의 명령과 응답에서 이에 대해 보여줍니다.

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

## 4. 그래프 작성

{{site.data.keyword.composeForJanusGraph}}에는 그래프를 작성하고, 열고, 닫기 위한 전용 그래프 팩토리가 있습니다. 이 팩토리(`ConfiguredGraphFactory`)를 사용하면 기본 스토리지 메커니즘에 대해 알 필요가 없으므로 이름을 지정하는 것만으로 새 그래프가 작성됩니다. 새 그래프 작성을 시작하십시오. 이 튜토리얼의 샘플 명령에서는 _mygraph_ 그래프를 호출합니다.

`def` 키워드를 사용하여 그래프 변수를 선언하십시오.

```
def graph=ConfiguredGraphFactory.create("mygraph")
```

{{site.data.keyword.composeForJanusGraph}} 그래프 이름에는 영숫자 문자와 밑줄 문자만 포함될 수 있습니다.
{: .tip}

## 5. 그래프 열기

그래프를 작성한 후 그래프에 대해 작업하려면 해당 그래프를 열어야 합니다. `ConfiguredGraphFactory`에 대해 `open()`을 호출하여 이를 수행합니다. 이제 그래프를 엽니다.

```
def graph=ConfiguredGraphFactory.open("mygraph")
```

이 시점에서 그래프가 지속될 수 있도록 오퍼레이션을 커미트해야 합니다.

```
graph.tx().commit()
```

지금 세션을 닫으면 다시 돌아올 때 새 그래프가 표시됩니다.

## 6. 그래프에 정점 추가

그래프에는 소프트웨어 개발자와 소프트웨어 개발자가 개발한 소프트웨어 조각에 대한 정보가 포함됩니다. 여기서는 "marko"라는 사용자와 "lop"라는 소프트웨어 조각을 추가합니다.

먼저 marko를 추가합니다.

```
def v1 = graph.addVertex(T.label, "person", "name", "marko", "age", 29)
```

성공한 경우 이 명령은 추가한 정점의 인덱스를 리턴합니다(`==>v[4304]`). 계속해서 두 번째 정점을 추가할 수 있습니다.

```
def v2 = graph.addVertex(T.label, "software", "name", "lop", "lang", "java")
```

다시 한 번 오퍼레이션을 커미트해야 합니다.

```
graph.tx().commit()
```

## 7. 그래프에서 정점 찾기

정점(예를 들어, 이름이 "marko"인 정점)을 찾으려면 먼저 그래프를 순회한 후 has() 메소드를 사용합니다. 그러면 정점의 ID가 리턴됩니다.

```
def g=graph.traversal()
g.V().has("name","marko")
```

이전 세션을 닫고 새 세션을 시작한 경우 변수 v1 및 v2에 정점을 재지정하려면 next()를 사용합니다.

```
def v1 = g.V().has("name","marko").next()
def v2 = g.V().has("name","lop").next()
```

이후 후속 Gremlin 명령에서 이러한 변수를 사용할 수 있습니다.

## 8. 에지 추가

이제 에지에 레이블 "created" 및 가중치 0.4를 지정하여 두 개의 정점 사이에 가중치가 부여된 에지를 추가할 수 있습니다.

값을 `0.4d`로 지정하여 숫자를 10진수로 캐스트해야 합니다.

```
v1.addEdge("created", v2, "weight", 0.4d)
```

다시, 변경사항을 커미트했는지 확인하십시오.

## 9. 그래프 조회

이제 그래프를 조회하여 Marko에서 작성한 소프트웨어의 이름을 찾을 수 있습니다.

```
g.V().has("name","marko").out("created").values("name")
```
