---

Copyright:
  years: 2017,2018
lastupdated: "2017-12-12"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Introdução ao Compose for JanusGraph
{: #getting-started-with-compose-for-janusgraph}

{{site.data.keyword.composeForJanusGraph_full}} é um banco de dados de gráfico escalável que é otimizado para armazenar e consultar dados altamente interconectados que são modelados como milhões ou bilhões de vértices e bordas. A recuperação simples e eficiente de dados por meio dessas estruturas complexas é ativada pela compatibilidade do Apache Tinkerpop(TM) do JanusGraph, que permite que você execute consultas eficientes que seriam difíceis ou impossíveis com um banco de dados relacional tradicional. O {{site.data.keyword.composeForJanusGraph}} torna o JanusGraph ainda melhor gerenciando-o para você, oferecendo um sistema de implementação fácil e escalável que entrega alta disponibilidade e redundância, backups contínuos automatizados e muito mais.
{:shortdesc}

Para obter uma visão geral do {{site.data.keyword.composeForJanusGraph}}, veja [Conceitos do JanusGraph](./janusgraph-concepts.html)

## Criando uma instância de serviço do Compose for JanusGraph

[Crie uma instância do {{site.data.keyword.composeForJanusGraph}}](https://console.bluemix.net/catalog/services/compose-for-janusgraph/).

Ao criar uma instância do serviço, assegure-se de escolher um nome para seu
serviço e um nome de credencial. Deixe o serviço desvinculado; é possível conectar um
aplicativo ao seu serviço mais tarde usando as credenciais que são fornecidas quando o
serviço é provisionado. Os vários valores de credencial e como usá-los em um aplicativo são detalhados em [Conectando um aplicativo {{site.data.keyword.cloud}}](./connecting-bluemix-app.html).

Quando você provisiona sua instância de serviço, é possível escolher os planos *Padrão* ou *Corporativo*. Com o plano *Corporativo*, é possível provisionar sua instância em um cluster disponível do {{site.data.keyword.composeEnterprise}}. O {{site.data.keyword.composeEnterprise}} fornece a segurança e o isolamento requeridos pela conformidade corporativa e usa rede dedicada para assegurar o desempenho dos bancos de dados implementados. Veja a [Documentação do Compose Enterprise](../ComposeEnterprise/index.html) para obter mais detalhes.

## Gerenciando o Compose for JanusGraph

É possível gerenciar seu serviço no painel de serviço. Aqui é possível localizar informações sobre o banco de dados do {{site.data.keyword.cloud_notm}} Compose e como conectar-se a ele. Também é possível:
- gerenciar seus backups
- alocar mais recursos para seu serviço
- mudar a senha do serviço
- use listas de desbloqueio para restringir o acesso a seus bancos de dados. 
Para obter mais informações, veja [Configurações](./dashboard-settings.html).

## Conectando-se ao Compose for JanusGraph

É possível se conectar ao seu serviço com as credenciais que são criadas com o serviço ou com as sequências de conexões e linha de comandos que são fornecidas na guia *Visão geral* de seu painel de serviço.

Se você deseja se conectar a seu serviço do {{site.data.keyword.cloud_notm}} externo, é possível usar as sequências de conexões fornecidas ou a linha de comandos. É possível descobrir mais em [Conectando-se ao JanusGraph](./connecting-external.html).

## Conectando um aplicativo {{site.data.keyword.cloud_notm}}

Para conectar um aplicativo {{site.data.keyword.cloud_notm}} a seu serviço, use as credenciais que são criadas com o serviço. É possível localizar informações sobre como conectar um aplicativo {{site.data.keyword.cloud_notm}} a um serviço {{site.data.keyword.composeForJanusGraph}} em [Conectando um aplicativo {{site.data.keyword.cloud_notm}}](./connecting-bluemix-app.html).

## Usando o Data Browser do JanusGraph

Explorar seus dados do gráfico por meio da linha de comandos pode ser uma tarefa complexa. O Data Browser for {{site.data.keyword.composeForJanusGraph}} combina um Gerador de Consultas fácil de usar com Cartões de resposta de consulta detalhados que exibem os resultados de suas consultas como uma visualização JSON interativa e como um gráfico visualizado. Para descobrir mais, leia [Usando o Data Browser do JanusGraph](./data-browser.html)
