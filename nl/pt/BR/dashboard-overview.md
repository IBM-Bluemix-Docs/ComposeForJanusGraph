---

Copyright:
  years: 2017,2018
lastupdated: "2017-11-15"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Visão geral do serviço

A página _Visão geral_ mostra informações sobre seu banco de dados do Compose do {{site.data.keyword.cloud}}. A visão geral inclui informações essenciais de identificação e o uso do recurso atual. Você também localizará uma seção para sequências de conexões que podem ser usadas com ferramentas ou fazer uso de ferramentas para se conectar a seu banco de dados.

## Detalhes da implementação

O painel _Detalhes da implementação_ mostra detalhes de seu serviço.

![Deployment Details](./images/janusgraph-deployment-details.png "A view of the Deployment Details panel")

### Tipo

O tipo de banco de dados que é oferecido pelo serviço e a versão do banco de dados que seu serviço usa. Se uma versão de banco de dados mais recente estiver disponível, uma notificação será exibida, junto a um link para a seção [Fazer upgrade da versão](/docs/services/ComposeForJanusGraph/dashboard-settings.html#upgrade-version) de seu painel de serviço.

### Nome

Um identificador interno para o serviço.

### Uso

O tamanho de seu banco de dados e a quantia de armazenamento fornecido por seu plano de serviço.

## Tarefas atuais

Fazer mudanças administrativas em seu serviço (como ajuste de escala ou execução de um backup manual) inicia uma tarefa. Enquanto uma tarefa estiver em execução, o painel _Tarefas atuais_ aparecerá na página _Visão geral_, mostrando o nome da tarefa e uma barra de progresso. Quando a tarefa for concluída, o painel _Tarefas atuais_ não aparecerá mais na página _Visão geral_.

## Sequências de conexões

As Sequências de conexões podem ser usadas por algumas bibliotecas do cliente e contêm todas as informações necessárias para outras bibliotecas se conectarem. É possível descobrir como usar uma Sequência de conexões para conectar-se a seu serviço em [Conectando um aplicativo externo](./connecting-external.html).

Você localizará cada Sequência de conexões para seu serviço em uma guia diferente no painel _Sequências de conexões_.

### Sessão

É possível usar um URI de sessão para obter um token de autorização que é válido por 60 minutos. É necessário usar o token fornecido ao fazer chamadas para a implementação usando os URIs de HTTPS ou Websocket.

### HTTPS

Esta é a sequência de conexões fundamental para uma implementação do JanusGraph. Para usar uma sequência de conexões HTTPS, é necessário fornecer as credenciais do usuário administrativo para se conectar ao servidor. Para obter detalhes sobre como usar HTTPs, veja [Criando e atravessando um gráfico usando HTTPs](./tutorial-https.html).

### Websocket

Os URIs de Websocket podem ser usados para estabelecer uma sessão de longa execução com a implementação do JanusGraph. Eles são prefixados como `wss:` para denotar que as conexões serão protegidas por HTTPS. Essas sequências de conexões requerem que a autenticação básica seja usada com as credenciais do usuário administrativo para se conectar ao servidor.

Há várias bibliotecas para JanusGraph que usam WebSockets como sua conexão com o servidor; para trabalhar com o {{site.data.keyword.composeForJanusGraph}}, elas devem ser capazes de executar autenticação básica e usar WSS (WebSockets seguros sobre TLS).

### Gremlin Console YAML

É possível usar uma das configurações fornecidas para se conectar ao {{site.data.keyword.composeForJanusGraph}} usando o Gremlin Console. Para obter detalhes sobre como usar o Gremlin Console YAML, veja [Criando e atravessando um gráfico usando o Gremlin Console](./tutorial-gremlin-console.html).


## API de administração de instância

É possível gerenciar o serviço {{site.data.keyword.composeForJanusGraph}} por meio da API do {{site.data.keyword.cloud_notm}} Compose.

### Terminal de base

O terminal base é composto pela região na qual o serviço reside e pelo ID da instância de serviço. Ela estará no início de cada terminal.

### ID de implementação

O ID de implementação é necessário para a maioria das chamadas e identifica a instância de implementação específica.

### Referência

Para obter mais documentação e referência para usar a API Compose do {{site.data.keyword.cloud_notm}}, ao longo de todos os serviços Compose do {{site.data.keyword.cloud_notm}}, leia [A API Compose do {{site.data.keyword.cloud_notm}}](https://www.compose.com/articles/the-ibm-cloud-compose-api/).
