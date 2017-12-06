---

Copyright:
  Years: 2017
lastupdated: "2017-09-13"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Conectando-se ao JanusGraph

O JanusGraph suporta conexões usando solicitações de HTTP ou por WebSockets. Todas as conexões são executadas por HTTPS e requerem autenticação. As solicitações baseadas em HTTP suportam a autenticação básica e do token. A não ser para uma chamada única na implementação do JanusGraph, o uso de WebSockets ou Autenticação do token resultará em um desempenho melhor. A autenticação básica é um processo caro que torna as solicitações lentas.

Para saber mais sobre como usar HTTPs e para aprender sobre como criar e abrir gráficos, veja [Criando e atravessando um gráfico usando HTTPS](./tutorial-https.html).
{: .tip}

A comunicação real entre o cliente e o banco de dados envolve o cliente enviar solicitações em Gremlin, um dialeto da linguagem Groovy customizada para consultar as estruturas de gráfico e o banco de dados responder a essas consultas. Também é possível enviar solicitações usando o Gremlin Console.

Para obter detalhes sobre como configurar, instalar, configurar e usar o Gremlin Console, veja [Criando e atravessando um gráfico usando o Gremlin Console](./tutorial-gremlin-console.html).
{: .tip}

## Solicitações de HTTP

Se você estiver usando Solicitações de HTTP para se conectar ao servidor, um único terminal poderá ser usado para se comunicar com o JanusGraph e será configurado para round-robin em todos os nós disponíveis na implementação. Ele inclui o nome do host e a porta.

É possível obter Sequências de conexões HTTPS de sua página de visão geral do painel de serviço. A sequência de conexões requer autenticação básica usando as credenciais do usuário administrativo para se conectar ao servidor.

Para conectar-se ao {{site.data.keyword.composeForJanusGraph}} com uma solicitação de HTTP, é necessário usar o método HTTP POST com o terminal e enviar um documento no formato JSON com uma única entrada com a chave "gremlin" e um valor da consulta do Gremlin que você deseja enviar. 

```json
{
    "gremlin": "a gremlin query would go here"
}
```

Gremlin é uma linguagem de passagem extensiva, mas um programa nela também pode ser tão simples quanto "1+1". Para passar isso como um script em um comando `curl`, a parte de dados de uma solicitação de POST é:

```
'{"gremlin" : "1+1" }'
``` 

Se você estiver enviando o mesmo script para o servidor várias vezes, será possível melhorar o desempenho fornecendo um dicionário de "ligações" opcional com um mapa de variáveis que estão disponíveis para o script Gremlin. Isso ajuda com o desempenho, pois o servidor não precisará recompilar o script Gremlin.
{: .tip}

Informações adicionais sobre o protocolo HTTP podem ser localizadas na seção [Conectando via REST](http://tinkerpop.apache.org/docs/3.2.3/reference/#_connecting_via_rest) da Documentação do Apache Tinkerpop

Para enviar um script Gremlin para o terminal, você precisa ser autenticado. Há duas maneiras de ser autenticado com acesso à solicitação de HTTP: autenticação básica e autenticação do token.

## Autenticação básica

A autenticação básica envia seu nome do usuário e senha com a solicitação. Para consultas únicas, isso é razoável, mas mais do que isso e você começa a carregar o servidor com a complexidade de fazer a troca senha e validação. Com curl, é possível passar o nome do usuário e a senha com uma opção `-u`:

```shell
$ curl -XPOST -d '{"gremlin" : "1+1" }' "https://portal91-0.ianuspater.compose-3.composedb.com:17916" -u admin:PASSWORDHERE
```

Também é possível integrar o nome do usuário e a senha na URL de terminal. 

## Autenticação do token

Para obter solicitações de HTTP mais eficientes, é possível obter um token de sessão de outro terminal. Esse token é passado no cabeçalho para todas as solicitações subsequentes. Se você estiver executando mais de uma chamada no terminal HTTP, será possível melhorar o desempenho usando a autenticação do token.

Use uma das sequências de conexões no painel _Sessão_ da página de visão geral do painel de serviço. As sequências de conexões já incluem a chamada necessária para obter o token como um comando curl:

```shell
$ curl -XGET "https://admin:PASSWORDHERE@portal91-0.ianuspater.compose-3.composedb.com:17916/session"
```

O resultado JSON retornado contém o token de sessão:

```
{
  "token": "YWRtaW46MTQ5NjkyODM1NDgzMzpWYXlxMERsMWs0VmltdGo3WUpzMEwtN3prUi00TGxZR3J6LXZnbDVmN3lnPQ=="
}
```

O token é válido por 60 minutos
{: .tip}

Extraia o valor do token e, em seguida, inclua-o como o cabeçalho `Authorization` em quaisquer solicitações adicionais:

```shell
$ curl -XPOST -H "Authorization: Token YWRtaW46MTQ5NjkyODM1NDgzMzpWYXlxMERsMWs0VmltdGo3WUpzMEwtN3prUi00TGxZR3J6LXZnbDVmN3lnPQ==" -d '{"gremlin" : "1+1" }' "https://portal91-0.ianuspater.compose-3.composedb.com:17916"
```

Se você está executando isso por meio do shell e tem o comando [`jq`](https://stedolan.github.io/jq/) disponível, é possível configurar uma variável de ambiente executando um comando como este:

```shell
$ export JGTOKEN=`curl -s -XGET "https://admin:PASSWORDHERE@portal91-0.ianuspater.compose-3.composedb.com:17916/session" | jq -r .token`
```

E, em seguida, executar solicitações como esta:

```shell
$ curl -XPOST -H "Authorization: Token $JGTOKEN" -d '{"gremlin" : "1+1" }' "https://portal91-0.ianuspater.compose-3.composedb.com:17916"
```

## Respostas de HTTP JSON

As respostas para as consultas a seguir estão na forma de um documento JSON com um valor de ID da solicitação, um objeto de status e um objeto de resultado. Aqui está um exemplo de uma resposta:

```json
{
  "requestId": "1bf4d1d4-eba4-484f-a94b-3cfa85df3448",
  "status": {
    "message":"", "code":200, "attributes":{}
  }, "result":{
    "data":[ {
        "god1": "hercules",
        "god2": "nemean"
      },
      {
        "god1": "hercules",
        "god2": "hydra"
      }
    ], "meta":{}
  }
}
```
O `requestId` é um identificador exclusivo para a solicitação. O `status` contém uma mensagem legível, um código no estilo de status HTTP e outros atributos do status relatado. O `result` contém um objeto de dados que é estruturado de acordo com o que o código Gremlin executado retorna e uma seção meta para quaisquer informações extras que estão fora do escopo dos dados de resultado.

## Websockets

WebSockets são uma maneira eficiente de criar uma conexão bidirecional de longa duração sobre TCP e TLS que é ideal para sessões interativas entre aplicativos clientes e o servidor JanusGraph. Várias bibliotecas para JanusGraph usam WebSockets como sua conexão com o servidor; para trabalhar com o JanusGraph no Compose, elas devem ser capazes de executar autenticação básica e usar WSS, WebSockets seguros sobre TLS. 

É possível obter suas Sequências de conexões de Websockets da seção _Websocket_ da página de visão geral do painel de serviço. Os URIs podem ser usados para estabelecer uma sessão de longa execução com a implementação do JanusGraph e são prefixados com `wss:` para denotar que as conexões serão protegidas por HTTPS. Essas sequências de conexões requerem que a autenticação básica seja usada com as credenciais do usuário administrativo para se conectar ao servidor.

## Gremlin Console

O Console Gremlin é a ferramenta essencial para trabalhar com servidores ativados para Tinkerpop. O Gremlin Console usa a conectividade WebSockets, mas não usa a sintaxe de URI para se conectar; isso é em parte porque ele também precisa configurar outros elementos para trabalhar com a conexão.

Para configurar o Gremlin Console, você fornece um arquivo YAML com os detalhes apropriados nele. É possível localizar essa configuração no painel _Gremlin Console YAML_ na página de visão geral do painel de serviço.

Para obter detalhes sobre como configurar, instalar, configurar e usar o Gremlin Console, veja [Criando e atravessando um gráfico usando o Gremlin Console](./tutorial-gremlin-console.html).
