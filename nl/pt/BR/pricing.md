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

# Preços
{: #pricing}

## Configuração base
Um serviço {{site.data.keyword.composeForJanusGraph_full}} começa como um cluster de dois nós JanusGraph Engine, cada um com 512 MB de memória, que é igual a 2 unidades de recursos. JaunsGraph Storage é um cluster de três nós, em que cada nó tem 5 GB de armazenamento, que é igual a 5 unidades de recursos. O serviço _inclui_ replicação e alta disponibilidade, portanto, cada unidade do JanusGraph Engine e o preço por unidade _incluem_ o custo dos recursos em ambos os nós do mecanismo. Da mesma forma, cada unidade do JanusGraph Storage e preço por unidade inclui o custo dos recursos nos três nós de armazenamento.

A configuração base também inclui dois portais HAProxy para acesso, HTTPS e lista de aplicativos confiáveis IP. Eles têm 64 MB de memória cada um.

### Total
A configuração de serviço de base tem um preço definido. Consulte o ladrilho de catálogo no {{site.data.keyword.cloud_notm}} para precificação base em sua moeda local. Por exemplo, o preço base em dólares americanos é US$ 116/mês. Isso inclui as 5 unidades para o cluster do JanusGraph Storage a US$ 90/mês e as 2 unidades para o JanusGraph Engine a US$ 26/mês.


## Opções de expansão
A descoberta da configuração ideal de memória e armazenamento variará entre os casos de uso e cargas de trabalho. Se você precisar de armazenamento adicional para conjuntos de dados grandes ou memória adicional para consultas complexas, poderá aumentar os recursos alocados para ambos, o armazenamento fornecido pelo cluster do JanusGraph Storage e a memória fornecida pelos nós do JanusGraph Engine. 

O aumento de clusters de armazenamento em unidades de 1 GB e o preço por unidade _incluem_ o custo para aumentar os recursos em todos os nós no cluster. O ajuste de escala de armazenamento está disponível na guia _Configurações_ do serviço.
 
O aumento de unidades de 256 MB de memória dos nós de mecanismo e o preço por unidade _incluem_ o custo para aumentar os recursos em ambos os nós. Atualmente, o ajuste de escala de memória está disponível apenas por meio do contato de suporte.

### Total
Cada unidade adicional (1 GB) do JanusGraph Storage e cada unidade adicional de memória do JanusGraph Engine (256 MB) têm um preço por unidade que é listado em sua moeda local no quadro de catálogo do {{site.data.keyword.cloud_notm}} para o serviço. Em dólares americanos, cada unidade do JanusGraph Storage é US$ 18 e cada unidade do JanusGraph Engine é US$ 13. Conforme o tamanho _total_ de todos os serviços do {{site.data.keyword.composeForJanusGraph}} aumenta, o preço por unidade diminui, conforme mostrado na [precificação em camadas](#tiered-pricing).

## Precificação em camadas

### Precificação em camadas do {{site.data.keyword.composeForJanusGraph}} Storage

Número de unidades do JanusGraph Storage|Preço por unidade
----------|-----------
5 - 9 unidades|preço base por unidade -- US$ 18,00/Unidade do Storage
10 - 24 unidades|10% redução -- US$ 16,20/Unidade do Storage
25 - 49 unidades|20% redução -- US$ 14,40/Unidade do Storage
50 - 99 unidades|30% redução -- US$ 12,60/Unidade do Storage
100 - 499 unidades|40% redução -- US$ 10,80/Unidade do Storage
500 - 999 unidades|50% redução -- US$ 9,00/Unidade do Storage
1.000 - 4.999 unidades|60% redução -- US$ 7,20/Unidade do Storage
+ de 5.000 unidades|70% redução -- US$ 5,40/Unidade do Storage
{: caption="Tabela 1. Precificação em camadas do {{site.data.keyword.composeForJanusGraph}} Storage" caption-side="top"}

### Precificação em camadas de memória do {{site.data.keyword.composeForJanusGraph}}

Número de unidades do JanusGraph Engine|Preço por unidade
----------|-----------
2 - 9 unidades|preço base por unidade -- US$ 13,00/Unidade do Engine
10 - 24 unidades|10% redução -- US$ 11.70/Unidade do Engine
25 - 49 unidades|20% redução -- US$ 10,40/Unidade do Engine
50 - 99 unidades|30% redução -- US$ 9,10/Unidade do Engine
100 - 499 unidades|40% redução -- US$ 7,80/Unidade do Engine
500 - 999 unidades|50% redução -- US$ 6,50/Unidade do Engine
1.000 - 4.999 unidades|60% redução -- US$ 5,20/Unidade do Engine
+ de 5.000 unidades|70% redução -- US$ 3,90/Unidade do Engine
{: caption="Tabela 2. Precificação em camadas de memória do {{site.data.keyword.composeForJanusGraph}}" caption-side="top"}
