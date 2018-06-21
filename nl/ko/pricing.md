---

copyright:
  years: 2016,2018
lastupdated: "2018-03-27"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 가격 책정
{: #pricing}

## 기본 구성
{{site.data.keyword.composeForJanusGraph_full}} 서비스는 각각 512MB의 메모리가 있는 두 개의 JanusGraph Engine 노드 클러스터로 시작하며, 이는 리소스 2개 단위와 동일합니다. JaunsGraph Storage는 각 노드에 5GB의 스토리지가 있는 세 개의 노드로 이뤄진 클러스터이며 이는 리소스 5개 단위와 동일합니다. 이 서비스는 복제 및 고가용성 기능을 _포함_하므로 각 JanusGraph Engine 단위 및 단위당 가격은 두 엔진 노드 전체에 있는 리소스의 비용을 _포함_합니다. 마찬가지로 각 JanusGraph Storage 단위 및 단위당 가격은 세 스토리지 노드에 있는 리소스의 비용을 포함합니다.

기본 구성은 액세스, HTTPS 및 IP 화이트리스팅을 위한 두 개의 HAProxy 포털 또한 포함합니다. 각각에는 64MB의 메모리가 있습니다.

### 비용
기본 서비스 구성에는 설정된 가격이 있습니다. 사용자의 지역 통화로 되어 있는 기본 가격 책정은 {{site.data.keyword.cloud_notm}}의 카탈로그 타일을 참조하십시오. 예를 들면, 기본 가격은 US 달러로 $116/월입니다. 이는 5개의 JanusGraph Storage 클러스터 단위에 대한 $90/월 비용과 2개의 JanusGraph Engine 단위에 대한 $26/월 비용을 포함합니다.


## 확장 옵션
메모리 및 스토리지에 대한 최적 구성은 유스 케이스 및 워크로드에 따라 달라집니다. 대규모 데이터 세트를 위해 추가 스토리지가 필요하거나 복잡한 조회를 위해 추가 메모리가 필요한 경우에는 JanusGraph Storage 클러스터에서 제공하는 스토리지와 JanusGraph Engine 노드에서 제공하는 메모리에 할당되는 리소스를 늘릴 수 있습니다.  

스토리지 클러스터는 1GB 단위로 늘어나며 단위당 가격은 클러스터에 있는 모든 노드의 리소스를 늘리는 비용을 _포함_합니다. 스토리지 스케일링은 서비스의 _설정_ 탭에서 가능합니다.
 
엔진 노드는 256MB 메모리 단위로 늘어나며 단위당 가격은 두 노드 전체의 리소스를 늘리는 비용을 _포함_합니다. 현재 메모리 스케일링은 지원 팀 문의를 통해서만 가능합니다.

### 비용
JanusGraph Storage의 각 추가 단위(1GB)와 JanusGraph Engine 메모리의 각 추가 단위(256MB)에는 단위당 가격이 있으며, 이는 서비스에 대한 {{site.data.keyword.cloud_notm}} 카탈로그 타일에 사용자의 지역 통화로 나열되어 있습니다. US 달러로 각 JanusGraph Storage 단위는 $18이며 각 JanusGraph Engine 단위는 $13입니다. [단계별 가격 책정](#tiered-pricing)에 표시되어 있는 바와 같이, 사용자의 _총_ {{site.data.keyword.composeForJanusGraph}} 서비스 크기가 증가하면 단위당 가격은 감소합니다.

## 단계별 가격 책정

### {{site.data.keyword.composeForJanusGraph}} Storage 단계별 가격 책정

JanusGraph Storage의 단위 수|단위당 가격
----------|-----------
5 - 9개 단위|기본 단위당 가격 -- $18.00 USD/Storage 단위
10 - 24개 단위|10% 감소 -- $16.20 USD/Storage 단위
25 - 49개 단위|20% 감소 -- $14.40 USD/Storage 단위
50 - 99개 단위|30% 감소 -- $12.60 USD/Storage 단위
100 - 499개 단위|40% 감소 -- $10.80 USD/Storage 단위
500 - 999개 단위|50% 감소 -- $9.00 USD/Storage 단위
1,000 - 4,999개 단위|60% 감소 -- $7.20 USD/Storage 단위
5,000개 이상 단위|70% 감소 -- $5.40 USD/Storage 단위
{: caption="표 1. {{site.data.keyword.composeForJanusGraph}} Storage 단계별 가격 책정" caption-side="top"}

### {{site.data.keyword.composeForJanusGraph}} Memory 단계별 가격 책정

JanusGraph Engine의 단위 수|단위당 가격
----------|-----------
2 - 9개 단위|기본 단위당 가격 -- $13.00 USD/Engine 단위
10 - 24개 단위|10% 감소 -- $11.70 USD/Engine 단위
25 - 49개 단위|20% 감소 -- $10.40 USD/Engine 단위
50 - 99개 단위|30% 감소 -- $9.10 USD/Engine 단위
100 - 499개 단위|40% 감소 -- $7.80 USD/Engine 단위
500 - 999개 단위|50% 감소 -- $6.50 USD/Engine 단위
1,000 - 4,999개 단위|60% 감소 -- $5.20 USD/Engine 단위
5,000개 이상 단위|70% 감소 -- $3.90 USD/Engine 단위
{: caption="표 2. {{site.data.keyword.composeForJanusGraph}} Memory 단계별 가격 책정" caption-side="top"}
