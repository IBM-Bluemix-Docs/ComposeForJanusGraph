---

copyright:
  years: 2017,2018
lastupdated: "2017-12-12"
---

# JanusGraph 개념

JanusGraph는 그래프 데이터베이스입니다. 데이터베이스 내에서 여러 그래프를 작성, 조회 및 수정할 수 있습니다. JanusGraph는 Apache Tinkerpop 스택을 기반으로 빌드되며 순회, 명령 및 조회를 위해 Gremlin 언어를 사용합니다.

JanusGraph에 대해 자세히 보려면 [JanusGraph 문서](http://docs.janusgraph.org/latest/index.html)를 참조하십시오.

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="//www.youtube.com/embed/zTaoMWv6lnE?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

[IBM Compose for JanusGraph 학습 센터](http://ibm.biz/janusgraph-learning)에서 {{site.data.keyword.composeForJanusGraph}}에 대한 더 많은 동영상을 볼 수 있습니다.
{: .tip}

## 그래프 데이터베이스에 대한 소개

간단히 말해 그래프는 사이에 에지가 있는 정점 세트입니다. 정점은 그래프의 기본 단위이며 일부 나눌 수 없는 오브젝트를 나타냅니다. 실질적인 측면에서는 개인, 위치 또는 오브젝트일 수 있습니다.  정점에는 오브젝트에 대해 설명하는 특성이 있을 수 있습니다. 

에지는 정점 간의 관계를 표시하는 두 개의 정점 간의 연결입니다. 에지에는 다중성, 방향 및 특성이 있을 수 있습니다.

비그래프 데이터베이스에서 엔티티 간의 관계는 필드에서 공유 키를 통해 표시되거나 JOIN을 통해 작성되는 데이터베이스의 보조 기능입니다. 즉, 다음 관계에서는 더 일반적인 조회에서 관계를 어셈블하는 작업을 수행하는 여러 조회 또는 애플리케이션이 필요할 수 있습니다.

그래프 데이터베이스에서는 관계가 데이터 모델의 기본 컴포넌트이며 정점에서 에지로, 에지에서 정점 등으로 순회하는 것은 모델 내에서 데이터를 조회하기 위한 기본 메커니즘입니다. 이로 인해 그래프 데이터베이스는 "밴드 X를 좋아하고 친구가 있는 모든 사람 또는 브랜드 Y를 구매한 친구의 친구"와 같은 관계를 포함하는 조회에 훨씬 더 적합합니다. 

JanusGraph 내에서 각 그래프에는 고유 이름이 있습니다. 그래프를 작성하고, 열고, 수정하고, 조회하려면 Gremlin 언어를 사용합니다. Gremlin 조회는 원하는 결과를 제공하기 위해 일부 방식으로 그래프를 조작하거나 순회할 수 있는 명령으로 구성됩니다.

## 정점

정점은 오브젝트를 나타내는 그래프 요소입니다. 정점 작성 시 정점에는 그래프 엔진에서 설정된 ID와 사용자 정의된 레이블이 있습니다. 정점은 인덱스화되고 조회될 수 있는 특성을 추가로 저장할 수 있습니다.

정점을 작성하고 조회하는 방법에 대한 자세한 정보와 정점에 대한 기타 구현 세부사항은 [Tinkerpop/Gremlin 문서](http://tinkerpop.apache.org/docs/3.2.3/reference/#_the_graph_structure)를 참조하십시오.

## 에지

에지는 관계를 나타내는 그래프 요소입니다. 에지를 작성하기 위해 사용자가 수신/발신 정점 및 레이블을 제공합니다. 그래프 엔진에서 에지에 ID를 지정합니다. 에지는 인덱스화되고 조회될 수 있는 특성을 추가로 저장할 수 있습니다.

에지를 작성하고 조회하는 방법에 대한 자세한 정보와 에지에 대한 기타 구현 세부사항은 [Tinkerpop/Gremlin 문서](http://tinkerpop.apache.org/docs/3.2.3/reference/#_the_graph_structure)를 참조하십시오.

## 특성

특성은 에지 또는 정점에 대해 설명하거나, 이름을 지정하거나, 에지 또는 정점과 연관된 키:값 쌍입니다. 에지와 정점 모두에 여러 특성이 있을 수 있습니다. 예를 들어, 정점이 '사람' 유형이면 해당 점점에 '이름' 및 '나이' 특성을 지정할 수 있습니다.

이름이 동일한 모든 정점을 가져오는 것과 같이 해당 특성으로 정점 및 에지를 검색할 수 있도록 특성을 인덱스화할 수 있습니다.

다른 가중치가 부여된 여러 에지를 통한 정점의 그래프 순회의 경우 트립에 대한 총 가중치가 계산될 수 있도록 에지에 '가중치'와 같은 특성이 있을 수 있습니다. 

## 순회

순회는 그래프의 구조를 분석하는 프로세스입니다. 순회는 에지, 정점 및 해당 특성에 대한 정보를 검색하고 리턴하는 프로세스입니다. 여러 순회 단계 및 알고리즘에 대해 알아보려면 [순회에 대한 Tinkerpop/Gremlin 문서](http://tinkerpop.apache.org/docs/3.2.3/reference/#traversal)를 참조하십시오.

## Gremlin

Gremlin에는 두 개의 파트(언어 및 서버)가 있습니다.

Gremlin(언어)은 그래프와 상호작용하고 그래프를 조회하는 데 사용하는 그래프 순회 언어입니다.

Gremlin(서버)은 로컬 또는 원격 Gremlin 언어 조회를 처리하는 서버의 스펙입니다. Gremlin 구현은 Apache Tinkerpop의 일부입니다.

Gremlin 언어 조회를 수행하려면 적절한 Gremlin 서버에 Gremlin 조회를 전송합니다. Compose에서는 JanusGraph 배치의 일부로 실행 중인 서버입니다.

## Apache TinkerPop

Apache TinkerPop은 오픈 소스 그래프 컴퓨팅 프레임워크입니다. TinkerPop은 데이터를 그래프로 모델링하고 Gremlin 언어 명령과 상호작용하여 그래프를 작성, 순회 및 조작하는 시스템입니다. 이러한 조각 간의 맞춤 방식에 대해 자세히 알아보려면 [그래프 시스템 통합](http://tinkerpop.apache.org/docs/3.2.3/reference/#_graph_system_integration)에서 TinkerPop/Gremlin 문서를 참조하십시오.
