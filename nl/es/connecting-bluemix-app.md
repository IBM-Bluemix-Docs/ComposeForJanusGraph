---

copyright:
  years: 2017,2018
lastupdated: "2018-05-08"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Conexión de una aplicación {{site.data.keyword.cloud_notm}}

Para conectar una aplicación {{site.data.keyword.cloud}} al servicio, utilice las credenciales de servicio que se crean cuando se suministra el servicio.

## Credenciales disponibles

Nombre de campo|Descripción
----------|-----------
`deployment_id`|Un identificador interno para el servicio, tal como se ha creado en Compose.
`name`|El nombre del despliegue de la base de datos.
`db_type`|El tipo de base de datos que ofrece el servicio; en este caso, `JanusGraph`.
`uri_cli`|Una línea de mandatos `CURL` que muestra cómo enviar una consulta gremlin con autenticación basada en señal.
`uri_cli_1`|Una línea de mandatos `CURL` secundaria que se conecta a la instancia de la base de datos.
`maps`|no se utiliza
`uri`|El URI `https` que se utiliza al conectarse al servicio, que incluye el nombre de host del servidor y el número de puerto al que se conecta. Consulte [Conexión de una aplicación externa](./connecting-external.html).
`uri_direct_1`|Un URI `https` secundario para conectar con el servicio.
`misc`|Un campo padre que contiene los campos `session`, `websocket` y `gremlin_console_yaml`.
`misc.session`| Un URI `https` que se utiliza para recuperar una señal de autenticación de 60 minutos que se puede utilizar para interactuar con el servicio. Consulte [Conexión de una aplicación externa - Autenticación con señal](./connecting-external.html#token-authentication).
`misc.websocket`|Los URI `wss` que se utilizan cuando se conecta con el servicio mediante Websockets. Consulte [Conexión de una aplicación externa - Websockets](./connecting-external.html#websockets).
`misc.gremlin_console_yaml`|Información de YAML que se utiliza para configurar la consola de Gremlin para conectar con el servicio.  Consulte [Conexión de una aplicación externa - Consola de Gremlin](./connecting-external.html#gremlin-console).
{: caption="Tabla 1. Credenciales de Compose for JanusGraph" caption-side="top"}

## Utilización de las credenciales

La aplicación se tiene que conectar a su servicio para utilizar las credenciales del servicio. En los siguientes pasos se muestra cómo hacerlo, utilizando Node.js como ejemplo.

### Conexión de la aplicación al servicio

Si ya tiene una aplicación {{site.data.keyword.cloud_notm}}, puede conectarla a un servicio desde la cuenta de {{site.data.keyword.cloud_notm}}:

1. Inicie una sesión en su cuenta de {{site.data.keyword.cloud_notm}}
2. En el panel de control, seleccione la aplicación que desea conectar al servicio.
3. En el panel de conexiones de la página _Visión general_ de la aplicación, pulse **Conectar nuevo** o **Conectar existente** para conectarse a un servicio {{site.data.keyword.cloud_notm}} nuevo o existente.

  - Si ha pulsado **Conectar existente**, se muestra una lista de servicios disponibles. Seleccione el servicio al que desea conectar la app y pulse **Conectar**.
  - Si ha pulsado **Conectar nuevo**, siga los pasos para suministrar una nueva instancia de servicio. Cuando seleccione un servicio del catálogo para suministrarlo, el campo _Conectar a_ se llenará automáticamente con el nombre de la aplicación.

Si tiene una aplicación que no ha cargado en {{site.data.keyword.cloud_notm}}, puede enlazar la aplicación al servicio antes de cargarla en {{site.data.keyword.cloud_notm}}: 

1. Inicie una sesión en su cuenta de {{site.data.keyword.cloud_notm}}
2. Descargue e instale la herramienta [CLI de Cloud Foundry](https://github.com/cloudfoundry/cli)
3. Conéctese a {{site.data.keyword.cloud_notm}} en la herramienta de línea de mandatos y siga las indicaciones para iniciar la sesión.

  ```
  $ cf api https://api.ng.bluemix.net
  $ cf login
  ```

4. Abra el archivo `manifest.yml` de la aplicación.

  - Cambie el valor `host` por uno que sea exclusivo. El host que elija determinará el subdominio del URL de la aplicación: `<host>.mybluemix.net`.
  - Cambie el valor `name`. El valor que elija será el nombre de la app que aparecerá en el panel de control de {{site.data.keyword.cloud_notm}}.

5. Actualice el valor `services` en `manifest.yml` para que coincida con el nombre del servicio. El archivo `manifest.yml` tendrá el siguiente aspecto:

  ```
  ---
  applications:
  - name:    compose-janusgraph-sample-service
    host:    compose-janusgraph-sample-service
    memory:  256M
    services:
      - compose-janusgraph-sample-app
  ```

### Acceso y utilización de las credenciales

La aplicación tiene que analizar las variables de entorno de Cloud Foundry. En Node.js puede hacerlo con el paquete [cfenv](https://www.npmjs.com/package/cfenv).

1. Instale el paquete con `--save` para actualizar `package.json`:

  ```
  npm install --save cfenv
  ```

2. Extraiga las credenciales de {{site.data.keyword.composeForJanusGraph_full}} de las variables de entorno de Cloud Foundry.

  ```javascript
  const cfenv = require('cfenv');
  const appenv = cfenv.getAppEnv();

  // Dentro del entorno de la aplicación (appenv) hay un objeto services
  const services = appenv.services;

  // El objeto services es una correlación nombrada por servicio, extraiga la correspondiente a JanusGraph
  let janusgraph_services = services["compose-for-janusgraph"];

  // Ahora tomaremos el primer servicio JanusGraph enlazado y extraeremos su objeto credentials
  let credentials = janusgraph_services[0].credentials;
  ```

  Ahora puede utilizar las credenciales del servicio. Por ejemplo, para obtener un URI de sesión que pueda utilizar para obtener una señal de autenticación:

  ```javascript
  let jgurl = credentials.misc.session[0];
  ```

3. Envíe por push la app en {{site.data.keyword.cloud_notm}}. Cuando se envíe la app por push, se enlazará automáticamente al servicio.

  ```
  $ cf push
  ```

Para obtener más información sobre cómo utilizar aplicaciones con servicios de {{site.data.keyword.cloud_notm}}, consulte [Adición de un servicio a la app](https://console.{DomainName}/docs/services/reqnsi.html#add_service).
