---

copyright:
  years: 2017
lastupdated: "2017-09-13"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Creación y cruce de un gráfico mediante la consola de Gremlin

Este tema le ayudará a empezar a trabajar con su primera base de datos {{site.data.keyword.composeForJanusGraph_full}}. En esta guía de aprendizaje utilizaremos la consola de Gremlin para enviar mandatos al servidor de JanusGraph.

## 1. Instalar Gremlin

Para instalar la consola de Gremlin:

1. Asegúrese de que tiene una versión reciente de Java instalada.
2. Descargue [Gremlin Console versión 3.2.3](https://archive.apache.org/dist/tinkerpop/3.2.3/apache-tinkerpop-gremlin-console-3.2.3-bin.zip).
3. Desempaquete el archivo descargado en un directorio de trabajo.
4. Mediante la línea de mandatos o un terminal, vaya al directorio raíz de la consola de Gremlin y compruebe la descarga con el mandato `bin/gremlin.sh`:

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

5. Configure la consola de Gremlin. Encontrará el YAML de la consola de Gremlin en la página *Visión general* del servicio {{site.data.keyword.composeForJanusGraph}}. Guarde una de las configuraciones en un archivo en el directorio `conf`. En este ejemplo, vamos a guardarlo como `conf/compose.yaml`.
 
## 2. Conectar con Compose for JanusGraph

Para verificar la conexión, vuelva a ejecutar `bin/gremlin.sh`. A continuación, utilice el mandato `:remote` para conectar con el servidor tinkerpop:

```text
:remote connect tinkerpop.server conf/compose.yaml session
```

Este mandato indica a Gremlin que `conecte` con algo que es `tinkerpop.server` utilizando la configuración de `conf/compose.yaml`. También vamos a pasar otro argumento, `session`, para habilitar el soporte de sesiones.

Escriba `:help :remote` para ver las opciones del mandato `:remote`.
{: .tip}

Si la conexión se establece correctamente, verá un aviso de SSL y un mensaje que confirmará la conexión.

```text
WARN  org.apache.tinkerpop.gremlin.driver.Cluster  - SSL configured without a trustCertChainFile and thus trusts all certificates without verification (not suitable for production)
==>Configured portal93-4.ianuspater.compose-3.composedb.com/159.8.153.216:18045-[2378d0de-0a93-4330-b8d8-848667f5b117]
gremlin>
```

## 3. Enviar mandatos al servidor.

La consola de Gremlin no reenvía nada el servidor remoto automáticamente; para enviar un mandato al servidor, debe preceder el mandato con el prefijo `:>`. También puede redirigir todos los mandatos de la consola al servidor remoto con `:remote console`. Esto se muestra en la siguiente serie de mandatos y respuestas:

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

## 4. Crear el gráfico

{{site.data.keyword.composeForJanusGraph}} tiene una fábrica de gráficos dedicada para crear, abrir y cerrar gráficos. Utilizar esta fábrica, `ConfiguredGraphFactory`, significa que no tiene que conocer los mecanismos de almacenamiento subyacentes, de modo que crear un nuevo gráfico es cuestión de asignarle un nombre. Empiece por crear un nuevo gráfico. En los mandatos de ejemplo de esta guía de aprendizaje, el gráfico se llama _mygraph_.

Utilice la palabra clave `def` para declarar la variable graph.

```
def graph=ConfiguredGraphFactory.create("mygraph")
```

Los nombres de gráficos de {{site.data.keyword.composeForJanusGraph}} solo pueden incluir caracteres alfanuméricos y el carácter de subrayado.
{: .tip}

## 5. Abrir el gráfico

Cuando haya creado un gráfico, para trabajar con el mismo debe abrirlo. Para ello debe llamar al método `open()` en `ConfiguredGraphFactory`. Vamos a abrir nuestro gráfico:

```
def graph=ConfiguredGraphFactory.open("mygraph")
```

En este momento tenemos que confirmar la operación para que el gráfico se mantenga:

```
graph.tx().commit()
```

Si cerramos nuestra sesión ahora, el nuevo gráfico seguirá estando cuando volvamos.

## 6. Añadir vértices al gráfico

Nuestro gráfico contendrá información sobre los desarrolladores de software y las piezas de software que han desarrollado. Vamos a añadir una persona llamada "marko" y una pieza de software llamada "lop".

Primero vamos a añadir a marko.

```
def v1 = graph.addVertex(T.label, "person", "name", "marko", "age", 29)
```

Si este mandato se ejecuta correctamente, devolverá el índice del vértice que hemos añadido, `==>v[4304]`, y podremos continuar añadiendo el segundo vértice.

```
def v2 = graph.addVertex(T.label, "software", "name", "lop", "lang", "java")
```

Una vez más, debemos confirmar las operaciones.

```
graph.tx().commit()
```

## 7. Buscar un vértice en el gráfico

Para buscar un vértice, por ejemplo el denominado "marko", primero cruzamos el gráfico y luego utilizamos el método has(), que devuelve el identificador del vértice.

```
def g=graph.traversal()
g.V().has("name","marko")
```

Para volver a asignar nuestros vértices a las variables v1 y v2, suponiendo que hayamos cerrado nuestra sesión antigua y abierto una nueva, utilizamos next ().

```
def v1 = g.V().has("name","marko").next()
def v2 = g.V().has("name","lop").next()
```

Podemos utilizar estas variables en mandatos Gremlin siguientes.

## 8. Añadir un borde

A continuación añadiremos un borde ponderado entre los dos vértices, al que asignaremos la etiqueta "created" y la ponderación 0.4.

Observe que tiene que indicar que el número es decimal, especificando el valor como `0.4d`.

```
v1.addEdge("created", v2, "weight", 0.4d)
```

Nuevamente, asegúrese de confirmar el cambio.

## 9. Consultar el gráfico

Ahora puede consultar el gráfico para encontrar el nombre de cualquier software que haya creado Marko.

```
g.V().has("name","marko").out("created").values("name")
```
