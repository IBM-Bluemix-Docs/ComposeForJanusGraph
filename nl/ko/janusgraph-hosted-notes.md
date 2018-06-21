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

# 호스팅된 JanusGraph 참고

{{site.data.keyword.composeForJanusGraph_full}}는 호스팅된 서비스이므로 안전한 배치를 위한 조치는 전용 서버에서 실행되는 JanusGraph와는 몇 가지 부분에서 차이가 있습니다. 이 주제에서는 이러한 차이점 및 변형을 다룹니다. 이러한 변경사항 대부분은 Gremlin 언어 조회가 작동하는 방식에 영향을 줍니다.

## 샌드박스

Gremlin 조회가 시스템에 문제를 발생시킬 수 있는 기능에 액세스하지 않도록 하기 위해, 모든 조회는 샌드박스에서 실행됩니다. 이는 모든 함수 및 메소드 호출이 다음 정규식과 일치하는 시그니처를 포함해야 함을 의미합니다.

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

함수 화이트리스트의 항목과 일치하지 않는 함수 또는 메소드 호출을 실행하려 하면 오류가 발생합니다. 

```
[Static type checking] - Not authorized to call this method: ...
```

또한 이 샌드박스는 모든 변수가 안전 검사를 수행할 수 있도록 정적으로 선언될 것을 요구합니다. 이는 `def` 명령을 사용하여 수행할 수 있습니다.

## 세션

세션은 {{site.data.keyword.composeForJanusGraph}}에 대한 연결이 요청 간에 상태를 유지할 수 있게 해 줍니다. {{site.data.keyword.composeForJanusGraph}}에 대한 대부분의 연결 옵션에는 연관된 세션이 없습니다. 이는 이러한 연결 방법을 통해 전송되는 Gremlin 스크립트가 전적으로 독립적이어야 함을 의미합니다. 예를 들면 스크립트가 작업을 수행해야 하는 그래프 열기, 적절한 노드 조회, 이러한 노드로부터의 그래프 순회는 모두 하나의 요청 내에서 처리되어야 합니다. 이 제한사항은 HTTP 요청 및 WebSockets 연결에 모두 적용됩니다. 

이에 대한 예외는 `:remote connect`를 사용하여 Gremlin 콘솔에서 연결하는 경우입니다.

```
:remote connect tinkerpop.server conf/compose.yaml
```

위 명령을 사용하여 연결을 설정하는 경우에는 세션이 없으며, 연결이 HTTP 요청과 동일한 방식으로 작동합니다. 그러나 `session`을 인수로 추가하는 경우에는 세션 지원을 사용할 수 있게 됩니다.

```
:remote connect tinkerpop.server conf/compose.yaml session
```

세션을 사용하면 변수를 한 명령에서 정의하고 다른 명령에서 이를 참조할 수 있습니다.
{: .tip}

## 트랜잭션

기본 JanusGraph 데이터베이스에 대한 모든 변경사항은 트랜잭션에 캡슐화됩니다. 트랜잭션은 여기에 포함된 모든 변경사항을 영구적으로 적용하도록 커미트하거나, 영구적으로 적용하지 않도록 롤백할 수 있게 해 줍니다. 

HTTP 요청 또는 WebSocket을 통해 요청을 수행할 때, 데이터베이스에 쓰기를 수행해야 하는 변경을 수행하면 트랜잭션이 자동으로 시작됩니다. 이 트랜잭션은 요청이 완료되면 자동으로 커미트됩니다.

이에 대한 예외는 세션이 사용으로 설정된 상태로 Gremlin 콘솔을 사용하여 연결하는 경우입니다. 세션은 장시간 유지될 수 있으므로 사용자는 `graph.tx().commit()`를 호출하여 변경사항을 커미트할지 결정할 수 있으며, 여기서 `graph`는 변경사항을 포함하고 있는 열린 그래프입니다. 이는 또한 사용자가 `graph.tx().rollback()`을 호출하여 트랜잭션이 시작되기 전의 그래프 상태로 돌아갈 수 있음을 의미합니다. 

## 혼합 인덱스

현재 {{site.data.keyword.composeForJanusGraph}} 버전은 혼합 인덱스를 지원하지 않습니다.
