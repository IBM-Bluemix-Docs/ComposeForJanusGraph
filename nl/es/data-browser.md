---

copyright:
  years: 2017
lastupdated: "2017-09-01"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Utilización de JanusGraph Data Browser

Explorar los datos gráficos desde la línea de mandatos puede resultar una tarea compleja y puede resultar difícil formar cruces. Puede ser difícil visualizar los resultados, que se devuelven como texto o como salida JSON, en términos de relaciones gráficas comprensibles. Aquí es donde interviene el componente Browser for JanusGraph on Compose.

Data Browser for {{site.data.keyword.composeForJanusGraph_full}} combina un generador de consultas fácil de utilizar con potentes tarjetas de respuestas de consultas que se apilan bajo el generador. Cada tarjeta registra la consulta y muestra los resultados como una vista JSON interactiva y como un gráfico visualizado que se puede explorar, relativo a la vista JSON. Cada tarjeta le puede ayudar a definir mejor la consulta siguiente.

## Iniciación a Data Browser

El enlace con Data Browser se encuentra en la página _Visión general del panel de control_ del servicio. Pulse el enlace para cargar la interfaz en un nuevo separador del navegador.

Esta es una vista de Data Browser después de que se haya ejecutado una primera consulta.

![Una vista de Data Browser cuando se inicia](./images/databrowser_taggedFullscreenbrowser.png "Una vista de Data Browser cuando se inicia que muestra Query Builder, salida de la consulta en formatos JSON y visual y un mensaje de bienvenida a la guía de aprendizaje.")

Data Browser muestra Query Builder **(1)**, donde puede crear, editar y ejecutar sus consultas. Bajo Query Builder hay una tarjeta de respuesta de consulta **(2)**. Las tarjetas nuevas se insertan en la parte superior de la pila de tarjetas. La que antes era la primera tarjeta era la introducción interactiva para el navegador **(3)**, que se muestra cuando se inicia el navegador.

## Query Builder

Query Builder es un editor de varias líneas con resaltado de sintaxis que le ayuda a componer scripts Gremlin.

![Query Builder con una consulta de ejemplo](./images/databrowser_taggedquerybuilder.png "Query Builder con una consulta de ejemplo")

## Tarjetas de respuestas y pila de tarjetas de respuestas

Cada consulta genera una tarjeta de respuesta que contiene la consulta, una respuesta JSON y una visualización gráfica de los resultados de la consulta, si están disponibles. La parte superior de cada tarjeta muestra la consulta que se ha ejecutado.

![Cabecera de tarjeta de respuesta de ejemplo.](./images/databrowser_querybar.png)

La tarjeta muestra la consulta que se ha ejecutado **(1)**, el botón **Copiar** **(2)**, el botón **Contraer** y **Expandir** **(3)** y el botón **Cerrar** **(4)**.

A medida que ejecuta más consultas, cada una genera una nueva tarjeta de respuesta, que muestra la tarjeta de respuestas más reciente en primer lugar. Si la página crece mucho, o si detecta que el rendimiento de Data Browser se reduce, puede utilizar el botón **Contraer** para guardar algunas tramas. Si ya no necesita los resultados de una tarjeta, puede cerrarla por completo. El hecho de cerrar una tarjeta respuestas no suprime ningún dato gráfico.

## Respuesta de la consulta: el visor de JSON

El visor de JSON es una vista de texto con sintaxis resaltada de la respuesta. Las líneas están numeradas para ayudarle a moverse por los resultados. Se muestran pequeñas flechas en las partes en que el documento JSON está anidado. Puede pulsar sobre las flechas para visualizar las secciones anidadas:

![Contenido JSON desdoblado](./images/databrowser_queryresponse.png)

La vista JSON también incluye filtros que se pueden aplicar para gestionar la información que se muestra. Para seleccionar los filtros, pulse los botones **Etiqueta**, **Tipo** y **Propiedades**. Puede seleccionar varios filtros.

![Filtros del navegador en la operación](./images/databrowser_filteractions.png)

## Respuesta de la consulta: el visualizador

Si el resultado de la consulta se puede visualizar, la tarjeta muestra un gráfico en el que puede ver los vértices y los bordes de la respuesta de la consulta. Pulse sobre un vértice para ver sus propiedades. Puede pulsar y arrastrar los vértices para rotarlos y para dejarlos fijos en una posición.

Por ejemplo, utilizando el gráfico de la base de datos de ejemplo Gods, esta sería la consulta para encontrar los vértices que tienen la etiqueta 'God':

```groovy
def g=ConfiguredGraphFactory.open("example").traversal();
g.V().has(T.label, "god");
```

La consulta genera la siguiente tarjeta de respuesta y visualización, que muestran todos los vértices del gráfico que representan los dioses:

![Visualización gráfica de los vértices "god".](./images/databrowser_visualization.png)

La consulta siguiente genera un resultado que muestra los vértices 'god' junto con los bordes que salen de los mismos, y los vértices a los que van a parar estos bordes:

```groovy
def g=ConfiguredGraphFactory.open("example").traversal();
g.V().has(T.label, "god").outE().inV().path();
```

La visualización gráfica de los resultados de la consulta se parece a la siguiente:

![Visualización gráfica de los dioses, sus bordes 'out' y sus vértices 'in'.](./images/databrowser_edgesvertices.png)

### El mandato .path()

El visualizador representa los resultados de JSON que se muestran en el visor de JSON, por lo que solo se visualizan los vértices y los bordes devueltos. Si la ruta de la consulta solo atraviesa vértices, solo se devuelven vértices, pero si incluye bordes, estos se incluyen en los resultados. Hay varias formas de llenar los resultados con bordes. Un potente método consiste en utilizar la función `path()`. Cuando se añade a una consulta Gremlin, `path()` devuelve la ruta tomada para obtener los vértices en la respuesta de la consulta.

La documentación de Gremlin de [path-step](http://tinkerpop.apache.org/docs/current/reference/#path-step) contiene más información sobre la función `path()`.
{: .tip}

Por ejemplo, la consulta siguiente solo devuelve vértices:

```groovy
def g=ConfiguredGraphFactory.open("example").traversal();
g.V().outE().inV()
```

La visualización resultante también contiene únicamente vértices.

![resultado de ejemplo de una visualización gráfica sin bordes](./images/databrowser_visualization2.png)

Puede modificar la respuesta de la consulta añadiendo `path()` a la misma consulta.

```groovy
def g=ConfiguredGraphFactory.open("example").traversal();
g.V().outE().inV().path()
```

Ahora la consulta genera una respuesta que contiene vértices y bordes.

![Resultado de ejemplo de visualización gráfica que utiliza `.path()`.](./images/databrowser_visualization3.png)

## Manejo de los resultados 'null'

Algunos mandatos del navegador pueden devolver un resultado `null`. Esto puede suceder cuando el valor que devuelven no se puede serializar actualmente. El ejemplo más común es cualquier mandato o expresión que devuelva un gráfico, incluidos los métodos `open` y `create` de la clase `ConfiguredGraphFactory`. Aunque se muestra una respuesta `nulo`, los valores reales están intactos en JanusGraph y se pueden utilizar en una consulta. Cuando utilice `ConfiguredGraphFactory`, amplíe el mandato para que devuelva vértices y bordes para garantizar que se devuelve una respuesta JSON.
