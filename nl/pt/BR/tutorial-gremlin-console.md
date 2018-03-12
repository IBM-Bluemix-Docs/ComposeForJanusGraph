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

# Criando e atravessando um gráfico usando o Gremlin Console

Este tópico irá ajudá-lo a começar com o seu primeiro banco de dados do {{site.data.keyword.composeForJanusGraph_full}}. Para este tutorial, vamos usar o Gremlin Console para enviar comandos para o servidor JanusGraph.

## 1. Instalar o Gremlin

Para instalar o Gremlin Console:

1. Assegure-se de ter uma versão recente do Java instalada.
2. Faça download do [Gremlin Console versão 3.2.3](https://archive.apache.org/dist/tinkerpop/3.2.3/apache-tinkerpop-gremlin-console-3.2.3-bin.zip).
3. Descompacte o arquivo transferido por download em um diretório ativo.
4. Usando a linha de comandos ou o terminal, mude o diretório para o diretório raiz do Gremlin Console e verifique o download executando `bin/gremlin.sh`:

  ```text
  $ unzip -q  ~/Downloads/apache-tinkerpop-gremlin-console-3.2.3-bin.zip
  $ cd apache-tinkerpop-gremlin-console-3.2.3
  $ bin/gremlin.sh

          \,,,/
          (o o)
  -----oOOo-(3)-oOOo-----
  plugin activated: tinkerpop.server
  plugin activated: tinkerpop.utilities
  plugin activated: tinkerpop.tinkergraph
  gremlin> [CONTROL-D]                                                             $

  ```

5. Configure o Gremlin Console. É possível localizar o Gremlin Console YAML na página *Visão geral* de seu serviço {{site.data.keyword.composeForJanusGraph}}. Salve uma das configurações em um arquivo no diretório `conf`. Neste exemplo, vamos salvá-lo como `conf/compose.yaml`.
 
## 2. Conectar-se ao Compose for JanusGraph

Para verificar a conexão, execute `bin/gremlin.sh` novamente. Em seguida, use o comando `:remote` para se conectar ao servidor tinkerpop:

```text
:remote connect tinkerpop.server conf/compose.yaml session
```

Esse comando instrui o Gremlin a `conectar-se` a algo que é um `tinkerpop.server` usando a configuração em `conf/compose.yaml`. Também estamos passando outro argumento, `session`, para ativar o suporte à sessão.

Digite `:help :remote` para ver as opções para o comando `:remote`.
{: .tip}

Se a conexão for bem-sucedida, você verá um aviso SSL e uma mensagem para confirmar a conexão.

```text
WARN  org.apache.tinkerpop.gremlin.driver.Cluster  - SSL configured without a trustCertChainFile and thus trusts all certificates without verification (not suitable for production)
==>Configured portal93-4.ianuspater.compose-3.composedb.com/159.8.153.216:18045-[2378d0de-0a93-4330-b8d8-848667f5b117]
gremlin>
```

## 3. Enviar comandos para o servidor.

O console Gremlin não encaminha tudo para o servidor remoto automaticamente: para enviar um comando para o servidor, você precisa prefixar o comando com `:>`. Como alternativa, é possível redirecionar todos os comandos do console para o servidor remoto com `:remote console`. Isso é demonstrado pela série de comandos e respostas a seguir:

```text
$ bin/gremlin.sh                                                                   

        \,,,/
        (o o)
-----oOOo-(3)-oOOo-----
plugin activated: tinkerpop.server
plugin activated: tinkerpop.utilities
plugin activated: tinkerpop.tinkergraph
gremlin> 1+1
==>2
gremlin> :> 1+1
==>No remotes are configured.  Use :remote command.
gremlin> :remote connect tinkerpop.server conf/compose.yaml session
WARN  org.apache.tinkerpop.gremlin.driver.Cluster  - SSL configured without a trustCertChainFile and thus trusts all certificates without verification (not suitable for production)
==>Configured portal93-4.ianuspater.compose-3.composedb.com/159.8.153.216:18045-[016d7a68-cf70-450e-92eb-4e5e2a647b5b]
gremlin> :remote console
==>All scripts will now be sent to Gremlin Server - [portal93-4.ianuspater.compose-3.composedb.com/159.8.153.216:18045]-[016d7a68-cf70-450e-92eb-4e5e2a647b5b] - type ':remote console' to return to local mode
gremlin> 1+1
==>2
gremlin> 

```

## 4. Criar o gráfico

O {{site.data.keyword.composeForJanusGraph}} tem uma factory de gráfico dedicado para criar, abrir e fechar gráficos. Usando esta factory - `ConfiguredGraphFactory` - significa que não você não precisa saber sobre os mecanismos de armazenamento subjacentes, portanto criar um novo gráfico é apenas uma questão de nomeá-lo. Inicie criando um novo gráfico. Nos comandos de amostra neste tutorial, estamos chamando o nosso gráfico _mygraph_.

Use a palavra-chave `def` para declarar a variável de gráfico.

```
def graph=ConfiguredGraphFactory.create("mygraph")
```

Os nomes de gráficos do {{site.data.keyword.composeForJanusGraph}} podem incluir somente caracteres alfanuméricos e o caractere de sublinhado.
{: .tip}

## 5. Abrir o gráfico

Depois que você tiver criado um gráfico, é preciso abri-lo para trabalhar com ele. Você faz isso chamando `open()` em `ConfiguredGraphFactory`. Vamos abrir nosso gráfico:

```
def graph=ConfiguredGraphFactory.open("mygraph")
```

Neste momento, precisamos confirmar a operação para que o gráfico persista:

```
graph.tx().commit()
```

Se fecharmos a nossa sessão agora, o novo gráfico estará lá quando voltarmos.

## 6. Incluir vértices no gráfico

Nosso gráfico conterá informações sobre os desenvolvedores de software e as partes de software que eles desenvolveram. Vamos incluir uma pessoa chamada "marko" e uma parte de software chamada "lop".

Primeiro, vamos incluir marko.

```
def v1 = graph.addVertex(T.label, "person", "name", "marko", "age", 29)
```

Se bem-sucedido, esse comando retornará o índice do vértice que incluímos, `==>v[4304]`, e poderemos continuar para incluir o segundo vértice.

```
def v2 = graph.addVertex(T.label, "software", "name", "lop", "lang", "java")
```

Mais uma vez, precisamos confirmar as operações.

```
graph.tx().commit()
```

## 7. Localizar um vértice no gráfico

Para localizar um vértice, vamos considerar o nome "marko", nós primeiro atravessamos o gráfico e, em seguida, usamos o método has() e ele retorna o identificador do vértice.

```
def g=graph.traversal()
g.V().has("name","marko")
```

Para redesignar nossos vértices às variáveis v1 e v2, vamos considerar se fecharmos a nossa sessão antiga e iniciamos uma nova, usaremos next().

```
def v1 = g.V().has("name","marko").next()
def v2 = g.V().has("name","lop").next()
```

Podemos, então, usar essas variáveis em comandos subsequentes do Gremlin.

## 8. Incluir uma borda

Em seguida, é possível incluir uma borda ponderada entre os dois vértices, fornecendo à borda um rótulo de "created" e uma ponderação de 0,4.

Observe que você precisa converter o número como um decimal, especificando o valor como `0.4d`.

```
v1.addEdge("created", v2, "weight", 0.4d)
```

Novamente, certifique-se de confirmar a mudança.

## 9. Consultar o gráfico

Agora é possível consultar o gráfico para localizar o nome de qualquer software que o Marko criou.

```
g.V().has("name","marko").out("created").values("name")
```
