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

# Criando e atravessando um gráfico usando HTTPS

O {{site.data.keyword.composeForJanusGraph_full}} é fornecido com um banco de dados de gráfico de amostra, 'The Graph of the Gods'. Este tutorial usa esse banco de dados de amostra para ilustrar alguns conceitos do JanusGraph. Siga as etapas para criar, abrir e atravessar o gráfico, usando CURL para enviar comandos Gremlin. Para visualizar uma representação visual do gráfico, veja a documentação de [Introdução](http://docs.janusgraph.org/latest/getting-started.html) do JanusGraph. 

## 1. Conectar ao Compose for JanusGraph

Para se conectar ao serviço {{site.data.keyword.composeForJanusGraph}} usando CURL, você precisa usar uma Sequência de conexões. A Sequência de conexões inclui o nome do usuário, senha, servidor e número da porta que são necessários para se conectar ao seu serviço. É possível localizar isso na página _Visão geral_ na página de seu painel de serviço.

Antes de iniciar o tutorial, verifique se você pode se conectar usando a Sequência de conexões passando um comando Gremlin simples:

```
curl -XPOST -d '{"gremlin" : "1+1" }' "<CONNECTION STRING>"
```

Se a solicitação é bem-sucedida, o JanusGraph retorna uma resposta como esta:

```
{
  requestId":"d2fe6ac7-95d9-4c79-a254-123648cad54d",
  "status":{
    "message":"", "code":200, "attributes":{}
  }, "result":{
    "data":[2],
    "meta":{}
  }
}
```

Agora você está pronto para começar a trabalhar com The Graph of the Gods.

Inclua sua Sequência de conexões no término de cada um dos comandos CURL no tutorial.
{: .tip}

## 2. Criar o gráfico

O {{site.data.keyword.composeForJanusGraph}} tem uma factory de gráfico dedicado para criar, abrir e fechar gráficos. Usando esta factory - `ConfiguredGraphFactory` - significa que não você não precisa saber sobre os mecanismos de armazenamento subjacentes, portanto criar um novo gráfico é apenas uma questão de nomeá-lo. Inicie criando um novo gráfico, chamado _Example_.

O ambiente de simulação do JanusGraph requer que você declare todas as variáveis que estão sendo usadas. Para declarar variáveis, você usa a palavra-chave `def`. Por exemplo, para declarar a variável de gráfico:

```
curl -XPOST -d '{"gremlin": "def graph=ConfiguredGraphFactory.create(\"example\");0;"}'
```

Os nomes de gráficos do {{site.data.keyword.composeForJanusGraph}} podem incluir somente caracteres alfanuméricos e o caractere de sublinhado.
{: .tip}

Observe que o comando curl termina com `;0;`. Essa é uma solução alternativa, porque a API de solicitação de HTTP emite um erro (`{"message":"Cannot get namespace of root","Exception-Class":"java.lang.IllegalArgumentException"}`), embora ela tenha concluído a operação corretamente. Isso somente afeta o tipo de gráfico quando o código tenta retorná-lo.

## 3. Abrir o gráfico

Depois que você tiver criado um gráfico, é preciso abri-lo para trabalhar com ele. Você faz isso chamando `open()` no `JanusGraphConfiguredFactory`. Vamos abrir nosso gráfico 'example'.

```
curl -XPOST -d '{"gremlin": "def graph=ConfiguredGraphFactory.open(\"example\");0;"}'
```

## 4. Carregar The Graph of the Gods

Para carregar o gráfico de amostra, chama-se o método `GraphOfTheGods.loadWithoutMixedIndex()`, passando a ele seu gráfico 'exemplo' aberto e o modelo é criado para você.

```
curl -XPOST -d '{"gremlin": "def graph=ConfiguredGraphFactory.open(\"example\"); GraphOfTheGodsFactory.loadWithoutMixedIndex(graph,true);"}'
```

## 5. Preparar para atravessar o gráfico

Agora que você criou e abriu um gráfico e carregou alguns dados nele, é possível explorar o gráfico. Para mover-se ao redor e consultar um gráfico, você precisa de uma origem de passagem, que é obtida chamando `traversal()` no objeto de gráfico. Isso significa que o prefixo integral para qualquer comando seria:

```
curl -XPOST -d '{"gremlin": "def graph=ConfiguredGraphFactory.open(\"example\");def g=graph.traversal();0;"}'
```

O uso de `graph` para o gráfico e `g` para a origem de passagem é comum. Se você precisa somente da origem de passagem, é possível compactar isso para:

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal();0;"}'
```

## 6. Atravessar o gráfico para localizar Saturno

O gráfico Graph of the Gods contém um índice global de propriedades do nome. Esse tipo de índice é geralmente a primeiro etapa para chegar a um ponto específico no gráfico. Por exemplo, para localizar "saturn" no gráfico:

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next()"}'
```

Esse comando retorna o vértice que tem a propriedade `name:saturn`.

```
{
  "requestId":"34d349a7-fa8e-4268-8b8d-a8adc0056bf3",
  "status":{
    "message":"", "code":200, "attributes":{}
  }, "result":{
    "data":[ {
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
    ], "meta":{}
  }
}
```

## 7. Atravessar o gráfico para localizar os filhos de Saturno

Para promover o exemplo, usando o vértice "saturn" definindo-o em uma variável 'saturn', é possível perguntar quais vértices armazenam uma borda com o rótulo 'father' apontando para ela. Usar o método `in("father")` seleciona essas bordas recebidas e usar `values("name")` retorna o valor da propriedade do nome desse vértice.

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); ;g.V(saturn).in(\"father\").values(\"name\")"}'
```

A consulta retorna os nomes dos filhos de Saturno no gráfico, embora haja apenas o único filho: "jupiter".

```
{
  "requestId":"2ad6c01e-f724-4e0a-8895-4e8e368fb7ca",
  "status":{
    "message":"", "code":200, "attributes":{}
  }, "result":{
    "data":[
      "jupiter"
    ], "meta":{}
  }
}
```

Se você inclui a operação `in("father")` uma segunda vez, sua passagem o conduz para o neto do vértice de Saturno.

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); ;g.V(saturn).in(\"father\").in(\"father\").values(\"name\")"}'
```

Usando essa consulta, você descobre que o nome do neto de Saturno é 'hercules'.

## 8. Atravessar o gráfico - Local

No GraphOfTheGods, alguns limites têm uma propriedade "place", consistindo em latitude e longitude. Essa propriedade pode ser usada para eventos de localização geográfica. As bordas rotuladas "battled" usam a propriedade de local para representar onde as batalhas aconteceram. Para localizar todas as batalhas que aconteceram dentro de 50 km de Atenas (latitude: 37,97 e longitude: 23,72), procure as bordas do gráfico para qualquer valor da propriedade "place" que esteja dentro do raio dessas coordenadas.

É possível selecionar essas bordas usando `has("place", geoWithin(Geoshape.circle(37.97, 23.72, 50)))`.

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); g.E().has(\"place\", geoWithin(Geoshape.circle(37.97, 23.72, 50)))"}'
```

A consulta retorna o rótulo da borda para os vértices recebidos e de saída e seus identificadores. Para tornar a resposta mais imediatamente legível e para descobrir quem participou dessas duas batalhas, podemos usar `as()` para armazenar em arquivo stash os valores retornados como _god1_ e _god2_ e, em seguida, `select()` para recuperar esses valores, usando `by("name")` para extrair os nomes dos dois deuses envolvidos em cada batalha.

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); g.E().has(\"place\", geoWithin(Geoshape.circle(37.97, 23.72, 50))).as(\"source\").inV().as(\"god2\").select(\"source\").outV().as(\"god1\").select(\"god1\", \"god2\").by(\"name\")"}'
```

```
"result":{
  "data":[ {
      "god1":"hercules",
      "god2":"nemean"
    },
    {
      "god1":"hercules",
      "god2":"hydra"
    }
  ], "meta":{}
}
```

## 9. Atravessar o gráfico - Vértices

Em uma etapa anterior, você usou dois elementos `in()` para identificar que Hércules era neto de Saturno. A passagem também pode ser expressa como um loop, porque o Gremlin pode `repetir` um predicado:

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); def hercules=g.V(saturn).repeat(__.in(\"father\")).times(2).next()"}'
```

É possível executar operações avançadas que usam o vértice de Hércules como um ponto de início. Por exemplo, você pode:

- Atravesse as bordas rotuladas "father" e "mother" e determine o "type" de pais que Hércules tinha:

  ```
  curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); def hercules=g.V(saturn).repeat(__.in(\"father\")).times(2).next(); g.V(hercules).out(\"father\", \"mother\").label();" }'
  ```

- Acesse as bordas "battled" para ver com quem Hércules lutou

  ```
  curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); def hercules=g.V(saturn).repeat(__.in(\"father\")).times(2).next(); g.V(hercules).out(\"battled\").valueMap()" }' 
  ```

- Atravesse as bordas "battled" para ver com quem Hércules lutou mais de uma vez:

  ```
  curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); def hercules=g.V(saturn).repeat(__.in(\"father\")).times(2).next(); g.V(hercules).outE(\"battled\").has(\"time\", gt(1)).inV().values(\"name\")" }' 
  ```
  
