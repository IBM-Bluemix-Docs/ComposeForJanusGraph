---

copyright:
  years: 2016,2018
lastupdated: "2018-03-27"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Tarifas
{: #pricing}

## Configuración básica
Un servicio de {{site.data.keyword.composeForJanusGraph_full}} se inicia como un clúster de dos nodos de JanusGraph Engine, cada uno de los cuales con 512 MB de memoria, que es igual a 2 unidades de recursos. JaunsGraph Storage es un clúster de tres nodos donde cada nodo tiene 5 GB de almacenamiento, que es igual a 5 unidades de recursos. El servicio _incluye_ la réplica y la alta disponibilidad, por lo que cada unidad de JanusGraph Engine y el precio por unidad _incluye_ el coste de los recursos en cada uno de los nodos de motor. Asimismo, cada unidad de JanusGraph Storage y el precio por unidad incluye el coste de los recursos en los tres nodos de almacenamiento.

La configuración básica también incluye dos portales de HAProxy para el acceso, HTTPS, y la lista blanca de IP. Tienen 64 MB de memoria cada uno.

La configuración de servicio base tiene un precio establecido. Consulte los mosaicos del catálogo en {{site.data.keyword.cloud_notm}} para ver los precios básicos en su moneda local. Por ejemplo, el precio básico en dólares de EE.UU. es de 116 dólares USD/mes. Esto incluye las 5 unidades para el clúster de JanusGraph Storage a 90 dólares USD/mes y las 2 unidades para el JanusGraph Engine a 26 dólares USD/mes.

## Incremento de recursos

Encontrar la configuración óptima de memoria y almacenamiento variará entre casos de uso y cargas de trabajo. Si necesita más almacenamiento para grandes conjuntos de datos, o más memoria para las consultas complejas, puede aumentar los recursos asignados tanto al almacenamiento proporcionado por el clúster de JanusGraph Storage como a la memoria proporcionada por los nodos de JanusGraph Engine. 

Los clústeres de almacenamiento aumentan en unidades de 1 GB y el precio por unidad _incluye_ el coste para aumentar los recursos en todos los nodos del clúster. El escalado de almacenamiento está disponible en el separador _Valores_ del servicio.
 
El aumento en los nodos del motor de unidades de 256 MB de memoria y el precio por unidad _incluye_ el coste para aumentar los recursos en ambos nodos. Actualmente, el escalado de memoria solo está disponible poniéndose en contacto con el soporte.

## Cálculo del coste del despliegue
{: #tiered-pricing}

Cada unidad adicional (1 GB) de JanusGraph Storage y cada unidad adicional de memoria de JanusGraph Engine (256 MB) tiene un precio por unidad que aparece en la moneda local en el mosaico del catálogo de {{site.data.keyword.cloud_notm}} para el servicio. En dólares de EE.UU., cada unidad de JanusGraph Storage cuesta 18 dólares USD y cada unidad de JanusGraph Engine cuesta 13 dólares USD. A medida que aumente el tamaño _total_ de todos los servicios de {{site.data.keyword.composeForJanusGraph}}, el precio por unidad disminuye, tal como se muestra en [precio por niveles](#tiered-pricing).

Número de unidades de JanusGraph Storage|Precio por unidad
----------|-----------
De 5 a 9 unidades|precio básico por unidad -- 18,00 dólares USD/unidad de almacenamiento
De 10 a 24 unidades|10 % de reducción -- 16,20 dólares USD/unidad de almacenamiento
De 25 a 49 unidades|20 % de reducción -- 14,40 dólares USD/unidad de almacenamiento
De 50 a 99 unidades|30 % de reducción -- 12,60 dólares USD/unidad de almacenamiento
De 100 a 499 unidades|40 % de reducción -- 10,80 dólares USD/unidad de almacenamiento
De 500 a 999 unidades|50 % de reducción -- 9,00 dólares USD/unidad de almacenamiento
De 1.000 a 4.999 unidades|60 % de reducción -- 7,20 dólares USD/unidad de almacenamiento
Más de 5.000 unidades|70 % de reducción -- 5,40 dólares USD/unidad de almacenamiento
{: caption="Tabla 1. Precio por niveles de {{site.data.keyword.composeForJanusGraph}} Storage" caption-side="top"}

## Precio por niveles de {{site.data.keyword.composeForJanusGraph}} Memory

Número de unidades de JanusGraph Engine|Precio por unidad
----------|-----------
De 2 a 9 unidades|precio básico por unidad -- 13,00 dólares USD/unidad de motor
De 10 a 24 unidades|10 % de reducción -- 11,70 dólares USD/unidad de motor
De 25 a 49 unidades|20 % de reducción -- 10,40 dólares USD/unidad de motor
De 50 a 99 unidades|30 % de reducción -- 9,10 dólares USD/unidad de motor
De 100 a 499 unidades|40 % de reducción -- 7,80 dólares USD/unidad de motor
De 500 a 999 unidades|50 % de reducción -- 6,50 dólares USD/unidad de motor
De 1.000 a 4.999 unidades|60 % de reducción -- 5,20 dólares USD/unidad de motor
Más de 5.000 unidades|70 % de reducción -- 3,90 dólares USD/unidad de motor
{: caption="Tabla 2. Precio por niveles de {{site.data.keyword.composeForJanusGraph}} Memory" caption-side="top"}
