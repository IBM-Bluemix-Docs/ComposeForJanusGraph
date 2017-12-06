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

# Creación y cruce de un gráfico mediante HTTPS

{{site.data.keyword.composeForJanusGraph_full}} se suministra con una base de datos gráfica de ejemplo, 'The Graph of the Gods'. En esta guía de aprendizaje se utiliza esta base de datos de ejemplo para mostrar algunos conceptos sobre JanusGraph. Siga los pasos para crear, abrir y cruzar el gráfico, utilizando CURL para enviar mandatos Gremlin. Para obtener una representación visual del gráfico, consulte la documentación de [Iniciación](http://docs.janusgraph.org/latest/getting-started.html) de JanusGraph. 

## 1. Conectar con Compose for JanusGraph

Para conectarse al servicio {{site.data.keyword.composeForJanusGraph}} mediante CURL, debe utilizar una serie de conexión. La serie de conexión incluye nombre de usuario, contraseña, servidor y número de puerto que se necesitan para conectar con el servicio. Encontrará estos valores en la página _Visión general_ del panel de control del servicio.

Antes de empezar la guía de aprendizaje, compruebe que puede conectarse utilizando la serie de conexión; para ello, pase un mandato Gremlin sencillo:

```
curl -XPOST -d '{"gremlin" : "1+1" }' "<CONNECTION STRING>"
```

Si la solicitud se ejecuta correctamente, JanusGraph devuelve una respuesta como esta:

```
{
  requestId":"d2fe6ac7-95d9-4c79-a254-123648cad54d",
  "status":{
    "message":"",
    "code":200,
    "attributes":{}
  },
  "result":{
    "data":[2],
    "meta":{}
  }
}
```

Ahora está listo para empezar a trabajar con The Graph of the Gods.

Añada la serie de conexión al final de cada uno de los mandatos CURL de la guía de aprendizaje.
{: .tip}

## 2. Crear el gráfico

{{site.data.keyword.composeForJanusGraph}} tiene una fábrica de gráficos dedicada para crear, abrir y cerrar gráficos. Utilizar esta fábrica, `ConfiguredGraphFactory`, significa que no tiene que conocer los mecanismos de almacenamiento subyacentes, de modo que crear un nuevo gráfico es cuestión de asignarle un nombre. Empiece por crear un nuevo gráfico, denominado _Example_.

El recinto de pruebas de JanusGraph requiere que declare todas las variables que utilice. Para declarar variables, utilice la palabra clave `def`. Por ejemplo, para declarar la variable graph:

```
curl -XPOST -d '{"gremlin": "def graph=ConfiguredGraphFactory.create(\"example\");0;"}'
```

Los nombres de gráficos de {{site.data.keyword.composeForJanusGraph}} solo pueden incluir caracteres alfanuméricos y el carácter de subrayado.
{: .tip}

Observe que el mandato curl termina por `;0;`. Esta es una solución temporal, porque la API de solicitud HTTP emite un error (`{"message":"Cannot get namespace of root","Exception-Class":"java.lang.IllegalArgumentException"}`) aunque la operación haya finalizado correctamente. Esto solo afecta al tipo de gráfico cuando el código intenta devolverlo.

## 3. Abrir el gráfico

Cuando haya creado un gráfico, para trabajar con el mismo debe abrirlo. Para ello debe llamar al método `open()` en `JanusGraphConfiguredFactory`. Vamos a abrir nuestro gráfico 'example'.

```
curl -XPOST -d '{"gremlin": "def graph=ConfiguredGraphFactory.open(\"example\");0;"}'
```

## 4. Cargar The Graph of the Gods

Para cargar el gráfico de ejemplo, llame al método `GraphOfTheGods.loadWithoutMixedIndex()` y páselo al gráfico 'example' abierto; el modelo se creará automáticamente.

```
curl -XPOST -d '{"gremlin": "def graph=ConfiguredGraphFactory.open(\"example\"); GraphOfTheGodsFactory.loadWithoutMixedIndex(graph,true);"}'
```

## 5. Prepararse para cruzar el gráfico

Ahora que ha creado y abierto un gráfico y ha cargado algunos datos en el mismo, ya puede explorarlo. Para moverse por un gráfico y consultarlo, necesite un origen de cruce, que se obtiene mediante la llamada `traversal()` en el objeto del gráfico. Esto significa que el prefijo completo de cualquier mandato sería:

```
curl -XPOST -d '{"gremlin": "def graph=ConfiguredGraphFactory.open(\"example\");def g=graph.traversal();0;"}'
```

El uso de `graph` para el gráfico y de `g` para el origen de cruce es lo más común. Si solo necesita el origen de cruce, puede comprimir la sentencia del siguiente modo:

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal();0;"}'
```

## 6. Cruzar el gráfico para encontrar Saturno

El gráfico The Graph of the Gods contiene un índice global de propiedades de nombre. Este tipo de índice suele ser el primer paso para llegar a un determinado punto del gráfico. Por ejemplo, para buscar "saturn" en el gráfico:

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next()"}'
```

Este mandato devuelve el vértice que tiene la propiedad `name: saturn`.

```
{
  "requestId":"34d349a7-fa8e-4268-8b8d-a8adc0056bf3",
  "status":{
    "message":"",
    "code":200,
    "attributes":{}
  },
  "result":{
    "data":[
      {
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
    ],
  "meta":{}
  }
}
```

## 7. Cruzar el gráfico para encontrar los hijos de Saturno

Si utiliza el vértice "saturn" definiéndolo en una variable 'saturn', puede preguntar qué vértices almacenan un borde con la etiqueta 'father' que apunta dicho vértice. El método `in("father")` selecciona estos bordes de entrada y `values("name")` devuelve el valor de la propiedad name de dicho vértice.

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); ;g.V(saturn).in(\"father\").values(\"name\")"}'
```

La consulta devuelve los nombres de los hijos de Saturno en el gráfico, aunque solo hay un hijo: "jupiter".

```
{
  "requestId":"2ad6c01e-f724-4e0a-8895-4e8e368fb7ca",
  "status":{
    "message":"",
    "code":200,
    "attributes":{}
  },
  "result":{
    "data":[
      "jupiter"
    ],
    "meta":{}
  }
}
```

Si incluye la operación `in("father")` una segunda vez, el cruce le lleva al nieto del vértice Saturn.

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); ;g.V(saturn).in(\"father\").in(\"father\").values(\"name\")"}'
```

Con esta consulta encuentra que el nombre del nieto de Saturn es 'hercules'.

## 8. Cruzar el gráfico - Ubicación

En GraphOfTheGods, algunos bordes tienen una propiedad "place", que consta de latitud y longitud. Esta propiedad se puede utilizar para sucesos de geolocalización. Los bordes denominados "battled" (batalla) utilizan la propiedad place para representar dónde han tenido lugar las batallas. Para buscar todas las batallas que han tenido lugar en un radio de 50 km de Atenas (latitud: 37.97 y longitud: 23.72), busque en los bordes del gráfico cualquier valor de la propiedad "place" que quede dentro del radio de estas coordenadas.

Para seleccionar estos bordes, utilice `has("place", geoWithin(Geoshape.circle(37.97, 23.72, 50)))`.

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); g.E().has(\"place\", geoWithin(Geoshape.circle(37.97, 23.72, 50)))"}'
```

La consulta devuelve la etiqueta de borde de los vértices de entrada y de salida y sus identificadores. Para que la respuesta se pueda leer de inmediato y para buscar quién ha participado en estas dos batallas, podemos utilizar `as()` para ocultar los valores devueltos como _god1_ y _god2_ y luego `select()` para recuperar dichos valores, utilizando `by("name")` para extraer los nombres de los dos dioses que han participado en cada batalla.

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); g.E().has(\"place\", geoWithin(Geoshape.circle(37.97, 23.72, 50))).as(\"source\").inV().as(\"god2\").select(\"source\").outV().as(\"god1\").select(\"god1\", \"god2\").by(\"name\")"}'
```

```
"result":{
  "data":[
    {
      "god1":"hercules",
      "god2":"nemean"
    },
    {
      "god1":"hercules",
      "god2":"hydra"
    }
  ],
  "meta":{}
}
```

## 9. Cruzar el gráfico - Vértices

En un paso anterior ha utilizado dos elementos `in()` para identificar que Hercules es el nieto de Saturn. El cruce también se puede expresar como un bucle, porque Gremlin puede `repetir` (repeat) un predicado:

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); def hercules=g.V(saturn).repeat(__.in(\"father\")).times(2).next()"}'
```

Puede realizar operaciones avanzadas que utilicen el vértice Hercules como punto de partida. Por ejemplo, puede:

- Cruzar los bordes llamados "father" y "mother" y determinar el tipo ("type") de padres que tenía Hercules:

  ```
  curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); def hercules=g.V(saturn).repeat(__.in(\"father\")).times(2).next(); g.V(hercules).out(\"father\", \"mother\").label();" }'
  ```

- Cruzar los bordes "battled" para ver contra quién ha luchado Hercules:

  ```
  curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); def hercules=g.V(saturn).repeat(__.in(\"father\")).times(2).next(); g.V(hercules).out(\"battled\").valueMap()" }' 
  ```

- Cruzar los bordes "battled" para ver contra quién ha luchado Hercules más de una vez:

  ```
  curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); def hercules=g.V(saturn).repeat(__.in(\"father\")).times(2).next(); g.V(hercules).outE(\"battled\").has(\"time\", gt(1)).inV().values(\"name\")" }' 
  ```
  
