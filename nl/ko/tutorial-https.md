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

# HTTPS를 사용하여 그래프 작성 및 순회

{{site.data.keyword.composeForJanusGraph_full}}는 'The Graph of the Gods'라는 샘플 그래프 데이터베이스와 함께 제공됩니다. 이 튜토리얼에서는 해당 샘플 데이터베이스를 사용하여 일부 JanusGraph 개념에 대해 설명합니다. 단계에 따라 그래프를 작성하고, 열고, 순회하며 CURL을 사용하여 Gremlin 명령을 전송하십시오. 그래프의 시각적 표시를 보려면 JanusGraph [시작하기](http://docs.janusgraph.org/latest/getting-started.html) 문서를 참조하십시오. 

## 1. Compose for JanusGraph에 연결

CURL을 사용하여 {{site.data.keyword.composeForJanusGraph}} 서비스에 연결하려면 연결 문자열을 사용해야 합니다. 연결 문자열에는 서비스에 연결하는 데 필요한 사용자 이름, 비밀번호, 서버 및 포트 번호가 포함됩니다. 서비스 대시보드의 _개요_ 페이지에서 연결 문자열을 찾을 수 있습니다.

튜토리얼을 시작하기 전에 단순 Gremlin 명령을 전달하여 연결 문자열을 통해 연결할 수 있는지 확인하십시오.

```
curl -XPOST -d '{"gremlin" : "1+1" }' "<CONNECTION STRING>"
```

요청에 성공하면 JanusGraph가 다음과 같은 응답을 리턴합니다.

```
{
  requestId":"d2fe6ac7-95d9-4c79-a254-123648cad54d",
  "status":{
    "message":"",
    "code":200,
    "attributes":{}
  },
  "result":{
    "data":[2],
    "meta":{}
  }
}
```

이제 The Graph of the Gods에 대한 작업을 시작할 준비가 되었습니다.

튜토리얼에서 각 CURL 명령의 끝에 연결 문자열을 추가하십시오.
{: .tip}

## 2. 그래프 작성

{{site.data.keyword.composeForJanusGraph}}에는 그래프를 작성하고, 열고, 닫기 위한 전용 그래프 팩토리가 있습니다. 이 팩토리(`ConfiguredGraphFactory`)를 사용하면 기본 스토리지 메커니즘에 대해 알 필요가 없으므로 이름을 지정하는 것만으로 새 그래프가 작성됩니다. _Example_이라는 새 그래프를 작성하여 시작하십시오.

JanusGraph 샌드박스에서는 사용 중인 모든 변수를 선언해야 합니다. 변수를 선언하려면 `def` 키워드를 사용합니다. 예를 들어, graph 변수를 수행하려면 다음을 수행하십시오.

```
curl -XPOST -d '{"gremlin": "def graph=ConfiguredGraphFactory.create(\"example\");0;"}'
```

{{site.data.keyword.composeForJanusGraph}} 그래프 이름에는 영숫자 문자와 밑줄 문자만 포함될 수 있습니다.
{: .tip}

curl 명령은 `;0;`으로 끝납니다. HTTP 요청 API가 올바르게 오퍼레이션을 완료한 경우에도 오류(`{"message":"Cannot get namespace of root","Exception-Class":"java.lang.IllegalArgumentException"}`)를 생성하므로 이는 임시 해결책입니다. 이는 코드가 리턴하려고 시도할 때만 그래프 유형에 영향을 미칩니다.

## 3. 그래프 열기

그래프를 작성한 후 그래프에 대해 작업하려면 해당 그래프를 열어야 합니다. `JanusGraphConfiguredFactory`에서 `open()`을 호출하여 이를 수행합니다. 'example' 그래프를 엽니다.

```
curl -XPOST -d '{"gremlin": "def graph=ConfiguredGraphFactory.open(\"example\");0;"}'
```

## 4. Graph of the Gods 로드

샘플 그래프를 로드하기 위해 `GraphOfTheGods.loadWithoutMixedIndex()` 메소드를 열려 있는 'example' 그래프에 전달하여 이 메소드를 호출하면 모델이 작성됩니다.

```
curl -XPOST -d '{"gremlin": "def graph=ConfiguredGraphFactory.open(\"example\"); GraphOfTheGodsFactory.loadWithoutMixedIndex(graph,true);"}'
```

## 5. 그래프 순회 준비

그래프를 작성한 후 열고 일부 데이터를 그래프에 로드했으면 이제 그래프를 탐색할 수 있습니다. 그래프를 이동하고 조회하려면 그래프 오브젝트에 대해 `traversal()`을 호출하여 얻는 순회 소스가 필요합니다. 이는 명령의 전체 접두부가 다음과 같음을 의미합니다.

```
curl -XPOST -d '{"gremlin": "def graph=ConfiguredGraphFactory.open(\"example\");def g=graph.traversal();0;"}'
```

그래프의 경우 `graph`를 사용하고 순회 소스의 경우 `g`를 사용하는 것이 일반적입니다. 순회 소스만 필요한 경우 이를 다음으로 압축할 수 있습니다.

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal();0;"}'
```

## 6. 그래프를 순회하여 Saturn 찾기

Graph of the Gods 그래프에는 이름 속성의 글로벌 인덱스가 포함되어 있습니다. 이 유형의 인덱스는 종종 그래프의 특정 지점에 이르는 첫 번째 단계입니다. 예를 들어, 그래프에서 "saturn"을 찾으려면 다음을 수행합니다.

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next()"}'
```

이 명령은 `name: saturn` 특성이 있는 정점을 리턴합니다.

```
{
  "requestId":"34d349a7-fa8e-4268-8b8d-a8adc0056bf3",
  "status":{
    "message":"",
    "code":200,
    "attributes":{}
  },
  "result":{
    "data":[
      {
        "id":4184,
        "label":"titan",
        "type":"vertex",
        "properties":{
          "name":[
            {
              "id":"16z-388-sl",
              "value":"saturn"
            }
          ],
          "age":[
            {
              "id":"1l7-388-35x",
              "value":10000
            }
          ]
        }
      }
    ],
  "meta":{}
  }
}
```

## 7. 그래프를 순회하여 Saturn의 하위 찾기

추가적인 예제로 'saturn' 변수에서 정의하여 "saturn" 정점을 사용하면 이 정점을 가리키고 있는 레이블이 'father'인 에지를 저장하는 정점을 문의할 수 있습니다. `in("father")` 메소드를 사용하여 해당 수신 에지를 선택하고 `values("name")`을 사용하여 해당 정점의 이름 속성 값을 리턴합니다.

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); ;g.V(saturn).in(\"father\").values(\"name\")"}'
```

이 조회는 그래프에 있는 Saturn 하위의 이름을 리턴합니다(유일한 하위 "jupiter"만 있음).

```
{
  "requestId":"2ad6c01e-f724-4e0a-8895-4e8e368fb7ca",
  "status":{
    "message":"",
    "code":200,
    "attributes":{}
  },
  "result":{
    "data":[
      "jupiter"
    ],
    "meta":{}
  }
}
```

`in("father")` 오퍼레이션을 두 번 포함하면 순회 시 Saturn 정점의 손자로 이동합니다.

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); ;g.V(saturn).in(\"father\").in(\"father\").values(\"name\")"}'
```

이 조회를 사용하여 Saturn의 손자 이름이 'hercules'임을 알 수 있습니다.

## 8. 그래프 순회 - 위치

GraphOfTheGods에서 일부 에지에는 위도와 경도로 이루어진 "위치" 특성이 있습니다. 이 특성은 이벤트의 위치정보를 파악하는 데 사용될 수 있습니다. 레이블이 "battled"인 에지는 위치 특성을 사용하여 전쟁이 발생한 위치를 표시합니다. 아테네(위도: 37.97 및 경도: 23.72)의 50km 이내에서 발생한 모든 전쟁을 찾으려면 그래프의 에지에서 해당 좌표의 반경 이내에 있는 "위치" 특성의 값을 검색하십시오.

`has("place", geoWithin(Geoshape.circle(37.97, 23.72, 50)))`을 사용하여 해당 에지를 선택할 수 있습니다.

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); g.E().has(\"place\", geoWithin(Geoshape.circle(37.97, 23.72, 50)))"}'
```

이 조회는 수신 및 발신 정점에 대한 에지 레이블과 해당 ID를 리턴합니다. 응답을 더 즉각적으로 읽을 수 있도록 하고 이러한 두 개의 전쟁에 참여한 신을 찾으려면 `as()`를 사용하여 리턴된 값을 _god1_ 및 _god2_으로 보관한 후 `select()`를 사용하여 해당 값을 검색하고 `by("name")`을 사용하여 각 전쟁에 관련된 두 신의 이름을 추출합니다.

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); g.E().has(\"place\", geoWithin(Geoshape.circle(37.97, 23.72, 50))).as(\"source\").inV().as(\"god2\").select(\"source\").outV().as(\"god1\").select(\"god1\", \"god2\").by(\"name\")"}'
```

```
"result":{
  "data":[
    {
      "god1":"hercules",
      "god2":"nemean"
    },
    {
      "god1":"hercules",
      "god2":"hydra"
    }
  ],
  "meta":{}
}
```

## 9. 그래프 순회 - 정점

이전 단계에서는 두 개의 `in()` 요소를 사용하여 Hercules가 Saturn의 손자임을 식별했습니다. Gremlin이 술어를 `repeat`할 수 있으므로 순회를 루프로 표시할 수도 있습니다.

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); def hercules=g.V(saturn).repeat(__.in(\"father\")).times(2).next()"}'
```

Hercules 정점을 시작점으로 사용하는 고급 오퍼레이션을 수행할 수 있습니다. 예를 들어, 다음을 수행할 수 있습니다.

- 레이블이 "father" 및 "mother"인 에지를 순회하고 상위 Hercule의 "type"을 판별합니다.

  ```
  curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); def hercules=g.V(saturn).repeat(__.in(\"father\")).times(2).next(); g.V(hercules).out(\"father\", \"mother\").label();" }'
  ```

- "battled" 에지를 순회하여 Hercules와 싸운 신을 확인합니다.

  ```
  curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); def hercules=g.V(saturn).repeat(__.in(\"father\")).times(2).next(); g.V(hercules).out(\"battled\").valueMap()" }' 
  ```

- "battled" 에지를 순회하여 Hercules와 두 번 이상 싸운 신을 확인합니다.

  ```
  curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); def hercules=g.V(saturn).repeat(__.in(\"father\")).times(2).next(); g.V(hercules).outE(\"battled\").has(\"time\", gt(1)).inV().values(\"name\")" }' 
  ```
  
