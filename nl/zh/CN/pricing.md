---

copyright:
  years: 2016,2018
lastupdated: "2018-01-03"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 定价
{: #pricing}

## 基本配置
{{site.data.keyword.composeForJanusGraph_full}} 服务一开始在一个集群中有两个 JanusGraph Engine 节点，每个节点有 512 MB 内存，相当于 2 个资源单元。JaunsGraph Storage 是一个三节点集群，其中每个节点有 5 GB 存储空间，相当于 5 个资源单位。该服务_包含_复制和高可用性，因此每个 JanusGraph Engine 单元和单元单价都_包含_这两个引擎节点上资源的成本。与此类似，每个 JanusGraph Storage 单元和单元单价都包含三个存储器节点上资源的成本。

基本配置还包含两个 HAProxy 门户网站，用于访问、HTTPS 和 IP 白名单。每个门户网站有 64 MB 内存。

### 成本
基本服务配置具有固定价格。有关以本地货币为单位的基础定价，请查阅 {{site.data.keyword.cloud_notm}} 上的“目录”磁贴。例如，以美元为单位的基础价格是 116 美元/月。这包含 JanusGraph Storage 集群的 5 个单元（90 美元/月）和 JanusGraph Engine 的 2 个单元（26 美元/月）。


## 扩展选项
确定内存和存储空间的最佳配置将根据用例和工作负载而变化。如果服务需要更多存储空间或内存，您可以增加分配给 JanusGraph Storage 集群所提供存储量以及分配给 JanusGraph Engine 节点所提供内存的资源。 

存储器集群以 1 GB 为单位增加，单元单价_包含_增加集群中所有节点上资源的成本。在服务的_设置_选项卡上可以对存储器进行扩展。
 
引擎节点以 256 MB 内存为单位增加，单元单价_包含_增加两个节点上资源的成本。目前，只能通过联系支持人员来对内存进行扩展。

### 成本
每增加一个单元 (1 GB) 的 JanusGraph Storage 和一个单元的 JanusGraph Engine 内存 (256 MB) 都会有相应的单元单价，该单价会在服务的 {{site.data.keyword.cloud_notm}}“目录”磁贴上以本地货币列出。以美元为单位时，JanusGraph Storage 的每个单元的价格为 18 美元，JanusGraph Engine 的每个单元的价格为 13 美元。随着所有 {{site.data.keyword.composeForJanusGraph}} 服务的_总_大小增加，单元单价会下降，如下面的[分层定价](#tiered-pricing)所示。

## 分层定价

### {{site.data.keyword.composeForJanusGraph}} 存储空间分层定价

JanusGraph Storage 的单元数|单元单价
----------|-----------
5 - 9 个单元|基础单元单价 -- 18.00 美元/存储器单元
10 - 24 个单元|10% 折扣 -- 16.20 美元/存储器单元
25 - 49 个单元|20% 折扣 -- 14.40 美元/存储器单元
50 - 99 个单元|30% 折扣 -- 12.60 美元/存储器单元
100 - 499 个单元|40% 折扣 -- 10.80 美元/存储器单元
500 - 999 个单元|50% 折扣 -- 9.00 美元/存储器单元
1,000 - 4,999 个单元|60% 折扣 -- 7.20 美元/存储器单元
5,000 个及以上单元|70% 折扣 -- 5.40 美元/存储器单元
{: caption="表 1. {{site.data.keyword.composeForJanusGraph}} 存储空间分层定价" caption-side="top"}

### {{site.data.keyword.composeForJanusGraph}} 内存分层定价

JanusGraph Engine 的单元数|单元单价
----------|-----------
2 - 9 个单元|基础单元单价 -- 13.00 美元/引擎单元
10 - 24 个单元|10% 折扣 -- 11.70 美元/引擎单元
25 - 49 个单元|20% 折扣 -- 10.40 美元/引擎单元
50 - 99 个单元|30% 折扣 -- 9.10 美元/引擎单元
100 - 499 个单元|40% 折扣 -- 7.80 美元/引擎单元
500 - 999 个单元|50% 折扣 -- 6.50 美元/引擎单元
1,000 - 4,999 个单元|60% 折扣 -- 5.20 美元/引擎单元
5,000 个及以上单元|70% 折扣 -- 3.90 美元/引擎单元
{: caption="表 2. {{site.data.keyword.composeForJanusGraph}} 内存分层定价" caption-side="top"}
