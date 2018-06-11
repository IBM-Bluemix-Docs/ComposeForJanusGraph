---

Copyright:
  years: 2017,2018
lastupdated: "2018-03-27"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Iniciación a Compose for JanusGraph
{: #getting-started-with-compose-for-janusgraph}

{{site.data.keyword.composeForJanusGraph_full}} es una base de datos gráfica escalable optimizada para almacenar y consultar datos altamente interconectados diseñados como millones de vértices y bordes. La recuperación de forma simple y eficiente de datos de estas estructuras complejas se consigue mediante la compatibilidad con Apache Tinkerpop(TM) de JanusGraph, que le permite realizar consultas eficientes que resultarían muy difíciles o imposibles con una base de datos relacional tradicional. {{site.data.keyword.composeForJanusGraph}} convierte JanusGraph en un excelente método de gestión, que ofrece un sistema de despliegue escalable y sencillo que ofrece una alga disponibilidad y redundancia, copias de seguridad continuas automáticas y muchos más.
{:shortdesc}

Para obtener una visión general de {{site.data.keyword.composeForJanusGraph}}, consulte [Conceptos de JanusGraph](./janusgraph-concepts.html)

## Creación de una instancia de servicio de Compose for JanusGraph

[Cree una instancia de {{site.data.keyword.composeForJanusGraph}}](https://console.bluemix.net/catalog/services/compose-for-janusgraph/).

Al crear una instancia del servicio, asegúrese de que elige un nombre para el servicio y un nombre de credencial. Deje el servicio desenlazado; puede conectar una aplicación al servicio más adelante utilizando las credenciales proporcionadas cuando se proporcione el servicio. En el apartado [Conexión con una aplicación {{site.data.keyword.cloud}}](./connecting-bluemix-app.html) encontrará información sobre los distintos valores de credenciales y cómo utilizarlos en una aplicación.

Cuando suministre la instancia de servicio, puede elegir los planes *Estándar* o *Empresa*. Con el plan *Empresa*, puede suministrar la instancia en un clúster disponible de {{site.data.keyword.composeEnterprise}}. {{site.data.keyword.composeEnterprise}} proporciona la seguridad y nivel de aislamiento necesarios para el cumplimiento de las reglas empresariales y utiliza redes dedicadas para garantizar el rendimiento de las bases de datos desplegadas. Consulte la [documentación de Compose Enterprise](../ComposeEnterprise/index.html) para ver más detalles.

## Gestión de Compose for JanusGraph

Puede gestionar el servicio desde el panel de control del servicio. Aquí encontrará información sobre su base de datos de {{site.data.keyword.cloud_notm}} Compose y cómo conectarse a la misma. También puede:
- gestionar copias de seguridad
- asignar más recursos para el servicio
- cambiar la contraseña del servicio
- utilizar listas blancas para restringir el acceso a las bases de datos. 

Para obtener más información, consulte [Valores](./dashboard-settings.html).

{{site.data.keyword.composeForJanusGraph}} utiliza roles de Cloud Foundry para gestionar el acceso al servicio. Solo los usuarios con el rol de desarrollador pueden ver o utilizar el panel de control del servicio. Para obtener más información sobre los roles de Cloud Foundry, consulte las páginas [Acceso de Cloud Foundry](https://console.bluemix.net/docs/iam/cfaccess.html#cfaccess) y [Gestión del acceso de Cloud Foundry](https://console.bluemix.net/docs/iam/mngcf.html#mngcf).
{: .tip}

## Conexión con Compose for JanusGraph

Puede conectarse a su servicio utilizando las credenciales que se crean junto con el servicio, o con las series de conexión y la línea de mandatos que se proporcionan en el separador *Visión general* del panel de control del servicio.

Si desea conectar el servicio desde fuera de {{site.data.keyword.cloud_notm}}, puede utilizar las series de conexión proporcionadas o la línea de mandatos. Encontrará más información en [Conexión a JanusGraph](./connecting-external.html).

## Conexión de una aplicación {{site.data.keyword.cloud_notm}}

Para conectar una aplicación {{site.data.keyword.cloud_notm}} al servicio, utilice las credenciales que se crean junto con el servicio. Encontrará información sobre cómo conectar una aplicación {{site.data.keyword.cloud_notm}} a un servicio {{site.data.keyword.composeForJanusGraph}} en el apartado [Conexión de una aplicación {{site.data.keyword.cloud_notm}}](./connecting-bluemix-app.html).

## Utilización de JanusGraph Data Browser

Explorar los datos gráficos desde la línea de mandatos puede resultar una tarea compleja. Data Browser for {{site.data.keyword.composeForJanusGraph}} combina un generador de consultas fácil de utilizar con potentes tarjetas de respuestas de consultas que muestran los resultados de las consultas como una vista JSON interactiva y como un gráfico visualizado. Para obtener más información, consulte [Utilización de JanusGraph Data Browser](./data-browser.html)
