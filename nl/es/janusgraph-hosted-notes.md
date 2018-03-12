---
copyright:
  years: '2015, 2017'
lastupdated: '2017-11-21'
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Notas alojadas de JanusGraph

{{site.data.keyword.composeForJanusGraph_full}} es un servicio alojado, por lo que garantizar que los despliegues son seguros difiere en algunos casos de la ejecución de JanusGraph en un servidor dedicado. Este tema destaca las diferencias y variaciones. La mayoría de estos cambios afectan a cómo funcionan las consultas del lenguaje Gremlin.

## Recintos de pruebas

Para asegurarse de que las consultas Gremlin no accedan a la funcionalidad que podría comprometer el sistema, todas las consultas se ejecutan en un recinto de pruebas. Esto significa que todas las funciones y las llamadas a métodos deben tener una firma que coincida con una de las siguientes expresiones regulares:

```
java\.util.*
java\.lang\.Object.*
java\.lang\.Class <T extends java\.lang\.Object>#getSimpleName\(
java\.lang\.Iterable <T extends java\.lang\.Object>#iterator\(\)
java\.lang\.Boolean(?!#getBoolean\().*
java\.lang\.Double.*
java\.lang\.Float.*
java\.lang\.Integer(?!#getInteger\().*
java\.lang\.Long(?!#getLong\().*
java\.lang\.Math.*
java\.lang\.String(?!#intern\(\)).*
java\.lang\.StringBuilder.*
java\.lang\.Object#equals\(
java\.lang\.Object#hashCode\(
java\.util\.ArrayList#equals\(

org\.codehaus\.groovy\.runtime\.DefaultGroovyMethods.*
org\.codehaus\.groovy\.runtime\.StringGroovyMethods.*
org\.apache\.commons\.configuration\.MapConfiguration.*
org\.apache\.tinkerpop\.gremlin\.structure(?!\.io|\.Graph#toString\(|\.Graph#variables\(|\.Graph#configuration\(|\.Graph#io\(\)|\.util\.GraphFactory.*).*
org\.apache\.tinkerpop\.gremlin\.process\.traversal.*
org\.apache\.tinkerpop\.gremlin\.util\.Gremlin#version\(\)
org\.janusgraph\.core(?!\.JanusGraphFactory.*).*
org\.janusgraph\.graphdb(?!\.tinkerpop\.JanusGraphBlueprintsGraph#toString\(|\.tinkerpop\.JanusGraphBlueprintsGraph#variables\(|\.tinkerpop\.JanusGraphBlueprintsGraph#configuration\(|\.tinkerpop\.JanusGraphBlueprintsGraph#io\().*
org\.janusgraph\.example\.GraphOfTheGodsFactory(?!#create\(|#main\().*
org\.apache\.tinkerpop\.gremlin\.groovy\.plugin\.dsl\.credential\.CredentialGraph.*

Script\d+#.+\(?.+\)
groovy\.lang\.Closure <V extends java\.lang\.Object>#call\(?.+\)

org\.janusgraph\.util\.stats\.MetricManager#getRegistry\(\)
org\.janusgraph\.util\.stats\.MetricManager#INSTANCE
org\.apache\.tinkerpop\.gremlin\.util\.MetricManager#getRegistry\(\)
org\.apache\.tinkerpop\.gremlin\.util\.MetricManager#INSTANCE
```

Intentar ejecutar una función o una llamada a método no coincide con ninguna de las entradas de la lista blanca de funciones da lugar a un error: 

```
[Static type checking] - Not authorized to call this method: ...
```

El recinto de pruebas también requiere que todas las variables estén declaradas estáticamente para habilitar la comprobación de seguridad. Puede hacerlo con el mandato `def`.

## Sesiones

Una sesión permite una conexión a {{site.data.keyword.composeForJanusGraph}} para mantener el estado entre las solicitudes. La mayoría de las opciones de conexión a {{site.data.keyword.composeForJanusGraph}} no tienen sesiones asociadas con ellas. Esto significa que cualquier script Gremlin enviado a través de dichos métodos de conexión debe estar totalmente autocontenido. Por ejemplo, abrir cualquier gráfico con el que el script necesita trabajar, consultar a los nodos adecuados, y atravesar el gráfico de los nodos deberá manejarse en una solicitud. Esta restricción se aplica tanto a las solicitudes HTTP como a las conexiones WebSockets. 

La excepción a esto es cuando se hacen conectar desde la consola de Gremlin con `:remote connect`.

```
:remote connect tinkerpop.server conf/compose.yaml
```

Si realiza la conexión utilizando el mandato anterior no hay sesión, y la conexión funciona del mismo modo que las solicitudes HTTP. Pero si añade `session` como argumento, estará ahora habilitado el soporte de sesión.

```
:remote connect tinkerpop.server conf/compose.yaml session
```

Utilizando una sesión, puede definir una variable en un mandato y hacer referencia a la misma en otro mandato.
{: .tip}

## Transacciones

Todos los cambios en la base de datos JanusGraph subyacente se encapsulan en una transacción. Las transacciones permiten que se confirmen los cambios en ellas para hacerlas permanentes o retrotraerlas para asegurarse de que no. 

Cuando realiza una solicitud a través de una solicitud HTTP o de un WebSocket, una transacción se inicia automáticamente cuando se realiza un cambio que se escribirá en la base de datos. Dicha transacción también se confirma automáticamente cuando la solicitud se completa.

La excepción se produce cuando se conecta mediante la consola de Gremlin con sesión habilitada. La sesión puede ser de larga duración, por lo que depende del usuario confirmar los cambios llamando a `graph.tx().commit()`, donde `graph` es el gráfico abierto que contiene los cambios. Esto también significa que puede llamar a `graph.tx().rollback()` para volver al estado del gráfico antes de que comenzara la transacción. 

## Índices mixtos

La versión actual de {{site.data.keyword.composeForJanusGraph}} no soporta índices mixtos.
