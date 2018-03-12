---

copyright:
  years: 2017,2018
lastupdated: "2017-12-12"
---

# Conceitos do JanusGraph

JanusGraph é um banco de dados de gráfico. É possível criar, consultar e corrigir vários gráficos dentro do banco de dados. O JanusGraph é construído na pilha do Apache Tinkerpop e usa a linguagem Gremlin para passagem, comandos e consultas.

Para ler mais sobre o JanusGraph, veja a [Documentação do JanusGraph](http://docs.janusgraph.org/latest/index.html).

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="//www.youtube.com/embed/zTaoMWv6lnE?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

É possível localizar mais vídeos sobre o {{site.data.keyword.composeForJanusGraph}} no [IBM Compose for JanusGraph Learning Center](http://ibm.biz/janusgraph-learning).
{: .tip}

## Introdução aos bancos de dados de gráfico

No seu modo mais simples, um gráfico é um conjunto de vértices com bordas entre eles. O vértice, a forma singular de vértices, é a unidade fundamental do gráfico e representa algum objeto indivisível. Em termos práticos, isso pode ser uma pessoa, local ou objeto.  Ele pode ter propriedades para descrever o objeto. 

Uma borda é uma conexão entre dois vértices que expressam um relacionamento entre eles. Ela pode ter uma multiplicidade, direção e propriedades.

Em bancos de dados não de gráfico, os relacionamentos entre as entidades são uma função secundária do banco de dados, expressos por meio de chaves compartilhadas em campos ou criados por JOINs. Isso significa que os relacionamentos a seguir podem requerer múltiplas consultas ou aplicativos executando o trabalho de montagem de relacionamentos de consultas mais gerais.

Em bancos de dados de gráfico, o relacionamento é um componente primário do modelo de dados e atravessar do vértice para a borda para o vértice e além é o mecanismo primário para consultar os dados dentro do modelo. Isso torna os bancos de dados de gráfico muito mais apropriados para consultas que envolvem relacionamentos como "todas as pessoas que curtem a banda X e têm um amigo, ou amigo de um amigo, que compra a marca Y". 

No JanusGraph, cada gráfico tem um nome exclusivo. Para criar, abrir, modificar e consultar um gráfico, você usa a linguagem Gremlin. Uma consulta Gremlin é composta de comandos que podem manipular ou atravessar o gráfico de alguma forma para fornecer o resultado desejado.

## Vértices

Um vértice é um elemento gráfico que representa um objeto. Na criação do vértice, ele tem um identificador configurado pelo mecanismo de gráfico e um rótulo que é definido pelo usuário. Um vértice pode também armazenar propriedades que podem ser indexadas e consultadas.

Leia mais sobre como criar, consultar e outros detalhes de implementação para vértices, veja a [Documentação do Tinkerpop/Gremlin](http://tinkerpop.apache.org/docs/3.2.3/reference/#_the_graph_structure).

## Bordas

Uma borda é um elemento gráfico que representa um relacionamento. Para criar uma borda, o usuário fornece os vértices recebidos/de saída e um rótulo. A borda será designada a um identificador pelo mecanismo de gráfico. Uma borda pode também armazenar propriedades que podem ser indexadas e consultadas.

Leia mais sobre como criar, consultar e outros detalhes de implementação para bordas, veja a [Documentação do Tinkerpop/Gremlin](http://tinkerpop.apache.org/docs/3.2.3/reference/#_the_graph_structure).

## Propriedades

Uma propriedade é um par de chave:valor que descreve nomes ou está associado a uma borda ou vértice. As bordas e os vértices podem, ambos, ter múltiplas propriedades. Por exemplo, se o vértice é do tipo 'humano', podemos designar propriedades a esse vértice de 'nome' e 'idade'.

As propriedades podem ser indexadas para que os vértices e bordas possam ser recuperados por suas propriedades, como obter todos os vértices com o mesmo nome.

As propriedades de bordas podem ser coisas como 'ponderação', para que uma passagem de gráfico de vértices por múltiplas bordas de ponderações diferentes possa ter uma ponderação total calculada para a trip. 

## Passagem

Passagem é o processo de analisar estrutura de um gráfico. Passagem é o processo que descobre e retorna informações sobre bordas, vértices e suas propriedades. Para aprender sobre as diferentes etapas e algoritmos de Passagem, veja a [Documentação do Tinkerpop/Gremlin sobre passagem](http://tinkerpop.apache.org/docs/3.2.3/reference/#traversal).

## Gremlin

Há duas partes para Gremlin: a linguagem e o servidor.

Gremlin, a linguagem, é uma linguagem de passagem de gráfico que você usa para interagir e consultar seus gráficos.

Gremlin, o servidor, é uma especificação para um servidor que processa consultas de linguagem Gremlin locais ou remotas. Uma implementação dessa faz parte do Apache Tinkerpop.

Para executar uma consulta de linguagem Gremlin, você envia a consulta de Gremlin para o servidor Gremlin apropriado. No Compose, esse é um servidor em execução como parte de sua implementação do JanusGraph.

## Apache TinkerPop

Apache TinkerPop é uma estrutura de computação gráfica de software livre. TinkerPop é o sistema que modela os dados como um gráfico e interage com os comandos de linguagem Gremlin para criar, atravessar e manipular gráficos. Para ler mais sobre como essas partes se ajustam, veja a documentação do TinkerPop/Gremlin na [integração do sistema de gráfico](http://tinkerpop.apache.org/docs/3.2.3/reference/#_graph_system_integration).
