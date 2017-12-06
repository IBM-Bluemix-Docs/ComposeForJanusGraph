---

Copyright:
  Years: 2017
lastupdated: "2017-09-05"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Visão geral do serviço

A página _Visão geral_ mostra informações sobre seu banco de dados do {{site.data.keyword.cloud}} Compose. A visão geral inclui informações essenciais de identificação e o uso do recurso atual. Você também localizará uma seção para sequências de conexões que podem ser usadas com ferramentas ou fazer uso de ferramentas para se conectar a seu banco de dados.

## Detalhes da implementação

O painel _Detalhes da implementação_ mostra detalhes de seu serviço.

![Deployment Details](./images/janusgraph-deployment-details.png "A view of the Deployment Details panel")

### Tipo

O tipo de banco de dados que é oferecido pelo serviço e a versão do banco de dados que seu serviço usa.

### Nome

Um identificador interno para o serviço.

### Uso

O tamanho de seu banco de dados e a quantia de armazenamento fornecido por seu plano de serviço.


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
