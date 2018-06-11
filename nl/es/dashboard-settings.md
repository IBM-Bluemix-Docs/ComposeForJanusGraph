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
{:tip: .tip}

# Valores

Estas características le permiten adaptar el servicio {{site.data.keyword.composeForJanusGraph_full}} para que se ajuste mejor a sus necesidades y requisitos.


## Versión de actualización

Si hay una nueva versión de la base de datos disponible, aparece un menú desplegable que le permite seleccionar la versión a la que desea actualizar. Si no la hay, significa que el servicio está al nivel de la versión más reciente y el panel muestra la información de la versión actual.

![El panel Versión](./images/janusgraph-version-show.png "El panel Versión")


## Escalado de recursos

Si el servicio necesita más almacenamiento, o si desea reducir la cantidad de almacenamiento asignado a su servicio, puede hacerlo mediante el escalado de recursos.

1. Vaya a la página _Visión general_ del servicio.
2. En el panel _Detalles de despliegue_, pulse **Escalar recursos**. Se abre la página Escalar recursos.

    ![El panel Escalar recursos](./images/janusgraph-scale-show.png "El panel Escalar recursos")

3. Ajuste el graduador para aumentar o reducir el almacenamiento asignado al servicio {{site.data.keyword.composeForJanusGraph}}. Mueva el graduador a la izquierda para reducir la cantidad de almacenamiento o muévalo a la derecha para aumentar el almacenamiento.
4. Pulse **Despliegue de escalado** para activar el escalado y volver a la visión general del panel de control. 

Una vez finalizado el escalado, el panel _Detalles de despliegue_ se actualiza para mostrar el uso actual y el nuevo valor del almacenamiento disponible.

### Escalado de memoria

Si dispone de un conjunto de datos relativamente pequeño, pero necesita ejecutar consultas complejas, puede mantener el tamaño predeterminado de los nodos de almacenamiento, 5 GB, y aumentar la memoria de los nodos del motor para que el rendimiento de las consultas sea más eficiente. Para añadir memoria a los nodos de Janus Graph, póngase en contacto con el equipo de soporte.


## Actualizar contraseña

Es posible que tenga que cambiar la contraseña de su servicio. Para ello, utilice _Actualizar contraseña_. 

Aparecerá una nueva contraseña, generada aleatoriamente, o bien puede escribir su propia contraseña en el campo. Para volver a generar otra contraseña, pulse en el dado de la derecha del campo. 
  
![Actualización de la contraseña etcd](./images/janusgraph-update-password.png "El generador automático de contraseñas")

Pulse **Actualizar contraseña**. Se le solicitará que confirme el cambio. Pulse **Actualizar contraseña** en el diálogo para confirmar la nueva contraseña, o Cancelar para cancelar el cambio. El panel _Detalles de despliegue_ mostrará el progreso del trabajo en ejecución.

**Nota:** Cambiar la contraseña cambia las credenciales que usted y sus servicios utilizan para conectarse, e invalida la serie de conexión del servicio. También puede dar lugar a tiempo de inactividad.

### Actualización de aplicaciones conectadas
Cambiar la contraseña invalidará la serie de conexión existente y generará una nueva. Esto provocará una interrupción del servicio hasta que las aplicaciones conectadas se actualicen con la nueva serie de conexión. Para ello tendrá que especificar la nueva serie de conexión para las aplicaciones.

Encontrará más información sobre cómo conectar las aplicaciones en [Conexión de una aplicación {{site.data.keyword.cloud}}](./connecting-bluemix-app.html)
y en [Conexión de una aplicación externa](./connecting-external.html).


## Utilización de listas blancas

Si desea restringir el acceso a las bases de datos, puede crear una lista blanca de direcciones IP o rangos de direcciones IP específicos en el servicio. Cuando no hay direcciones IP en la lista blanca, esta se inhabilita y el despliegue aceptará conexiones procedentes de cualquier sistema de Internet.

![Lista blanca de IP](./images/janusgraph-whitelist-show.png "Los campos de lista blanca.")

### Direcciones IP
El campo *IP* acepta una sola dirección IPv4 o dirección IPv6 completa con o sin máscara de red. Sin máscara de red, las conexiones entrantes deben proceder exactamente de esa dirección IP. 

Tenga en cuenta que, aunque la entrada IP permite IPv6, actualmente no hay despliegues de Compose disponibles para redes IPv6, de modo que no se puede filtrar por estas direcciones.

### Máscaras de red
Para permitir una conexión procedente de un rango específico de direcciones IP, utilice una máscara de red. La dirección IP se debe especificar por completo cuando se utiliza una máscara de red. Esto significa especifica, por ejemplo, 192.168.1.0/24 en lugar de 192.168.1/24.

### Descripción
La *Descripción* puede ser cualquier texto significativo especificado por el usuario para identificar la entrada de la lista blanca: un nombre de cliente, el identificador del proyecto o un número de empleado, por ejemplo. El campo de descripción es obligatorio.

### Servicios de Compose
Las entradas de la lista blanca se añaden automáticamente a los servidores de Compose para permitir que se conecten.

### Eliminación
Para eliminar una dirección IP o una máscara de red de la lista blanca, pulse *Eliminar* junto a la entrada.
Si se eliminan todas las entradas de la lista blanca, la lista blanca se inhabilita y los portales de acceso TCP aceptarán todas las direcciones IP.
