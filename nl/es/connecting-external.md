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

# Conexión a JanusGraph

JanusGraph da soporte a conexiones mediante solicitudes HTTP o sobre WebSockets. Todas las conexiones se realizan sobre HTTPS y requieren autenticación. Las solicitudes basadas en HTTP dan soporte a la autenticación básica y mediante señal. Para cualquier otra acción que no sea una llamada puntual en el despliegue de JanusGraph, el uso de WebSockets o de autenticación mediante señal ofrecerá un mejor resultado. La autenticación básica es un proceso costoso que se ralentiza las solicitudes.

Para obtener más información sobre cómo utilizar HTTPS y para aprender a crear y abrir gráficos, consulte [Creación y cruce de un gráfico mediante HTTPS](./tutorial-https.html).
{: .tip}

La comunicación real entre el cliente y la base de datos implica que el cliente envíe solicitudes en Gremlin, un dialecto del lenguaje Groovy personalizado para consultar estructuras gráficas, y que la base de responda a las consultas. También puede enviar solicitudes mediante la consola de Gremlin.

Para obtener más información sobre cómo obtener, instalar, configurar y utilizar la consola de Gremlin, consulte [Creación y cruce de un gráfico mediante la consola de Gremlin](./tutorial-gremlin-console.html).
{: .tip}

## Solicitudes HTTP

Si utiliza solicitudes HTTP para conectar con el servidor, se puede utilizar un solo punto final para comunicar con JanusGraph y configurar el itinerario rotatorio por todos los nodos disponibles del despliegue. Incluye el nombre de host y el puerto.

Puede obtener las series de conexión de HTTPS desde la página de visión general del panel de control del servicio. La serie de conexión requiere autenticación básica mediante las credenciales de usuario de administración para conectar con el servidor.

Para conectarse a {{site.data.keyword.composeForJanusGraph}} con una solicitud HTTP, debe utilizar el método HTTP POST con el punto final y enviar un documento con formato JSON con una sola entrada con la clave "gremlin" y el valor de la consulta de Gremlin que desea enviar. 

```json
{
    "gremlin": "a gremlin query would go here"
}
```

Gremlin es un completo lenguaje de cruce, pero un programa en este lenguaje puede ser tan simple como "1+1". Para pasar esto como script en un mandato `curl`, la parte de datos de una solicitud POST es la siguiente:

```
'{"gremlin" : "1+1" }'
``` 

Si va a enviar el mismo script al servidor varias veces, puede mejorar el rendimiento proporcionando un diccionario de "enlaces" opcional con una correlación de las variables que están disponibles para el script gremlin. Esto ayuda a mejorar el rendimiento ya que el servidor no tiene que volver a compilar el script gremlin.
{: .tip}

Encontrará información adicional sobre el protocolo HTTP en la sección sobre [Conexión mediante REST](http://tinkerpop.apache.org/docs/3.2.3/reference/#_connecting_via_rest) de la documentación de Apache Tinkerpop.

Para enviar un script Gremlin al punto final, debe estar autenticado. Hay dos maneras de autenticarse con acceso de solicitud HTTP: la autenticación básica y la autenticación mediante señal.

## Autenticación básica

La autenticación básica envía su nombre de usuario y contraseña con la solicitud. Para consultas puntuales resulta razonable, pero si tiene que realizar muchas cargaría el servicio con la complejidad de tener que realizar el intercambio de contraseña y la validación. Con curl puede pasar el nombre de usuario y la contraseña con una opción `-u`:

```shell
$ curl -XPOST -d '{"gremlin" : "1+1" }' "https://portal91-0.ianuspater.compose-3.composedb.com:17916" -u admin:PASSWORDHERE
```

También puede incluir el nombre de usuario y la contraseña en el URL de punto final. 

## Autenticación mediante señal

Para conseguir solicitudes HTTP más eficiente, puede obtener una señal de sesión de otro punto final. Esta señal se pasa en la cabecera para todas las solicitudes posteriores. Si está realizando más de una llamada sobre el punto final HTTP, puede mejorar el rendimiento utilizando la autenticación mediante señal.

Utilice las series de conexión del panel _Sesión_ de la página de visión general del panel de control del servicio. Las series de conexión ya incluyen la llamada necesaria para obtener la señal como un mandato curl:

```shell
$ curl -XGET "https://admin:PASSWORDHERE@portal91-0.ianuspater.compose-3.composedb.com:17916/session"
```

El resultado JSON devuelto contiene la señal de sesión:

```
{
  "token": "YWRtaW46MTQ5NjkyODM1NDgzMzpWYXlxMERsMWs0VmltdGo3WUpzMEwtN3prUi00TGxZR3J6LXZnbDVmN3lnPQ=="
}
```

La señal es válida durante 60 minutos
{: .tip}

Extraiga el valor de la señal y luego inclúyalo en la cabecera `Authorization` de cualquier solicitud posterior:

```shell
$ curl -XPOST -H "Authorization: Token YWRtaW46MTQ5NjkyODM1NDgzMzpWYXlxMERsMWs0VmltdGo3WUpzMEwtN3prUi00TGxZR3J6LXZnbDVmN3lnPQ==" -d '{"gremlin" : "1+1" }' "https://portal91-0.ianuspater.compose-3.composedb.com:17916"
```

Si ejecuta esta acción desde el shell y tiene el mandato [`jq`](https://stedolan.github.io/jq/) disponible, puede definir una variable de entorno ejecutando un mandato como este:

```shell
$ export JGTOKEN=`curl -s -XGET "https://admin:PASSWORDHERE@portal91-0.ianuspater.compose-3.composedb.com:17916/session" | jq -r .token`
```

Luego puede ejecutar solicitudes como esta:

```shell
$ curl -XPOST -H "Authorization: Token $JGTOKEN" -d '{"gremlin" : "1+1" }' "https://portal91-0.ianuspater.compose-3.composedb.com:17916"
```

## Respuestas JSON HTTP

Las respuestas a las consultas tienen el formato de un documento JSON con un valor de ID de solicitud, un objeto de estado y un objeto de resultado. A continuación se muestra un ejemplo de respuesta:

```json
{
  "requestId": "1bf4d1d4-eba4-484f-a94b-3cfa85df3448",
  "status": {
    "message": "",
    "code": 200,
    "attributes": {}
  },
  "result": {
    "data": [
      {
        "god1": "hercules",
        "god2": "nemean"
      },
      {
        "god1": "hercules",
        "god2": "hydra"
      }
    ],
    "meta": {}
  }
}
```
El valor `requestId` es un identificador exclusivo de la solicitud. El valor `status` contiene un mensaje legible, un código de estilo de estado de HTTP y otros atributos del estado notificado. El valor `result` contiene un objeto de datos que está estructurado según lo que devuelve el código Gremlin ejecutado y una sección meta con información adicional que está fuera del ámbito de los datos de los resultados.

## Websockets

WebSockets constituye una forma eficaz de crear una conexión bidireccional de larga duración sobre TCP y TLS que resulta ideal para sesiones interactivas entre aplicaciones cliente y el servidor JanusGraph. Varias bibliotecas para JanusGraph utilizan WebSockets como método de conexión con el servidor; trabajar con JanusGraph en Compose, deben ser capaces de realizar una autenticación básica y de utilizar WSS, WebSockets seguro sobre TLS. 

Puede obtener series de conexión de Websockets de la sección _Websocket_ de la página de visión general del panel de control del servicio. Se pueden utilizar URI para establecer una sesión de larga ejecución con el despliegue de JanusGraph; van precedidos de `wss:` para indicar que las conexiones tendrán protección de HTTPS. Estas series de conexión requieren que se utilice la autenticación básica con las credenciales de usuario de administración para conectar con el servidor.

## Consola de Gremlin

La consola de Gremlin es la herramienta esencial para trabajar con servidores habilitados para Tinkerpop. La consola de Gremlin utiliza la conectividad WebSockets, pero no utiliza la sintaxis de URI para conectar; esto se debe en parte a que también necesita configurar otros elementos para trabajar con la conexión.

Para configurar la consola de Gremlin, debe proporcionan un archivo YAML que contenga los detalles adecuados. Encontrará esta configuración en el panel _YAML de consola de Gremlin_ de la página de visión general del panel de control del servicio.

Para obtener más información sobre cómo obtener, instalar, configurar y utilizar la consola de Gremlin, consulte [Creación y cruce de un gráfico mediante la consola de Gremlin](./tutorial-gremlin-console.html).
