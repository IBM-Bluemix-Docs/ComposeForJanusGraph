---

copyright:
  years: 2017,2018
lastupdated: "2017-12-12"
---

# Conceptos de JanusGraph

JanusGraph es una base de datos gráfica. Puede crear, consultar y corregir varios gráficos dentro de la base de datos. JanusGraph se basa en la pila Apache Tinkerpop y utiliza el lenguaje Gremlin para cruces, mandatos y consultas.

Para obtener más información sobre JanusGraph, consulte la [documentación de JanusGraph](http://docs.janusgraph.org/latest/index.html).

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="//www.youtube.com/embed/zTaoMWv6lnE?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Puede encontrar más vídeos sobre {{site.data.keyword.composeForJanusGraph}} en el [Centro de aprendizaje de IBM Compose for JanusGraph](http://ibm.biz/janusgraph-learning).
{: .tip}

## Introducción a las bases de datos gráficas

En su forma más simple, un gráfico es un conjunto de vértices con bordes entre ellos. El vértice, singular de vértices, es la unidad fundamental del gráfico y representa un objeto indivisible. En términos prácticos, puede ser una persona, una ubicación o un objeto.  Puede tener propiedades que describan el objeto. 

Un borde es una conexión entre dos vértices que expresa una relación entre ellos. Puede tener una multiplicidad, una dirección y propiedades.

En las bases de datos que no son gráficas, las relaciones entre entidades son una función secundaria de la base de datos, que se expresa a través de claves compartidas en los campos o se crean mediante sentencias JOIN. Esto significa que las relaciones siguientes puede requerir varias consultas o aplicaciones que realicen el trabajo de ensamblar las relaciones procedentes de consultas más generales.

En las bases de datos gráficas, la relación es un componente principal del modelo de datos y cruzar de un vértice a un borde a un vértice y más allá es el mecanismo principal para consultar los datos del modelo. Esto convierte a las bases de datos gráficas en un sistema más adecuado para consultas que implican relaciones del tipo "todas las personas a las que les gusta el grupo X y tienen un amigo, o un amigo de un amigo, que compra la marca Y". 

En JanusGraph, cada gráfico tiene un nombre exclusivo. Para crear, abrir, modificar y consultar un gráfico, se utiliza el lenguaje Gremlin. Una consulta Gremlin se compone de mandatos que pueden manipular o cruzar el gráfico de alguna forma para proporcionar el resultado que desea.

## Vértices

Un vértice es un elemento gráfico que representa un objeto. Cuando se crea, el vértice tiene un identificador definido por el motor del gráfico y una etiqueta definida por el usuario. Un vértice puede almacenar propiedades que se pueden indexar y que se pueden consultar.

Para obtener más información sobre cómo crear y consultar y para ver otros detalles de la implementación de vértices, consulte la [documentación de Tinkerpop/Gremlin](http://tinkerpop.apache.org/docs/3.2.3/reference/#_the_graph_structure).

## Bordes

Un borde es un elemento gráfico que representa una relación. Para crear un borde, el usuario proporciona los vértices de entrada y de salida y una etiqueta. El motor del gráfico asignará un identificador al borde. Un borde puede almacenar propiedades que se pueden indexar y que se pueden consultar.

Para obtener más información sobre cómo crear y consultar y para ver otros detalles de la implementación de bordes, consulte la [documentación de Tinkerpop/Gremlin](http://tinkerpop.apache.org/docs/3.2.3/reference/#_the_graph_structure).

## Propiedades

Una propiedad es un par clave:valor que describe, asigna nombre o está asociado a un borde o a un vértice. Tanto los bordes como los vértices pueden tener varias propiedades. Por ejemplo, si el vértice es de tipo 'human' (humano), podemos asignarle propiedades como 'name' (nombre) y 'age' (edad).

Las propiedades se pueden indexar para que los vértices y los bordes se puedan recuperar por sus propiedades, como por ejemplo para obtener todos los vértices con el mismo nombre.

Las propiedades de los bordes pueden ser cosas como 'weight' (ponderación), de modo que un cruce gráfico de vértices a través de varios bordes con distintas ponderaciones puede tener una ponderación total calculada para la ruta. 

## Cruce

El cruce es el proceso de analizar la estructura de un gráfico. El cruce es el proceso que descubre y devuelve información sobre bordes, vértices y sus propiedades. Para obtener más información sobre los distintos pasos y algoritmos de cruce, consulte la [documentación de Tinkerpop/Gremlin sobre cruces](http://tinkerpop.apache.org/docs/3.2.3/reference/#traversal).

## Gremlin

Hay dos partes de Gremlin: el lenguaje y el servidor.

Gremlin, el lenguaje, es un lenguaje de cruce gráfico que se utiliza para interactuar y consultar los gráficos.

Gremlin, el servidor, es una especificación para un servidor que procesa consultas locales o remotas del lenguaje Gremlin. Una implementación del mismo es parte de Apache Tinkerpop.

Para realizar una consulta en lenguaje Gremlin, debe enviar su consulta Gremlin al servidor Gremlin adecuado. En Compose, es un servidor que se ejecuta como parte del despliegue de JanusGraph.

## Apache TinkerPop

Apache TinkerPop es una infraestructura de cálculo gráfico de código abierto. TinkerPop es el sistema que modela los datos como gráfico y que interactúa con los mandatos del lenguaje Gremlin para crear, cruzar y manipular gráficos. Para obtener más información sobre cómo se combinan estos elementos, consulte en la documentación de TinkerPop/Gremlin el apartado sobre [integración del sistema gráfico](http://tinkerpop.apache.org/docs/3.2.3/reference/#_graph_system_integration).
