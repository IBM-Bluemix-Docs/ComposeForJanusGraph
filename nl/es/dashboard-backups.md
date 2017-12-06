---

copyright:
  years: 2017
lastupdated: "2017-10-16"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Copias de seguridad
{: #backups}

Puede crear y descargar copias de seguridad desde el separador _Copias de seguridad_ de la página *Gestionar* del panel de control del servicio. Dispone de copias de seguridad planificadas y manuales.

Las copias se crean haciendo una copia de seguridad de los nodos de la base de datos Scylla. Las copias de seguridad de Scylla se realizan mediante el programa de utilidad de instantánea de Scylla, que hace copia de seguridad de todos los archivos de datos del disco almacenados en el directorio de datos. La instantánea se puede ejecutar mientras las bases de datos están en línea.

## Visualización de las copias de seguridad existentes

Se planifican automáticamente copias de seguridad diarias de la base de datos. Para ver las copias de seguridad existentes:

1. Vaya a la página _Gestionar_ del panel de control del servicio.
2. Pulse **Copias de seguridad** en los separadores para abrir la página _Copias de seguridad_. Aparecerá una lista de las copias de seguridad disponibles, con las copias de seguridad más reciente en la parte superior de la lista:

  ![Copias de seguridad disponibles](./images/janusgraph-backups-show.png "Una lista de copias de seguridad disponibles, incluida una copia de seguridad pendiente")

Pulse en la fila correspondiente para ampliar las opciones para cualquier copia de seguridad disponible.![Opciones de copia de seguridad](./images/janusgraph-backups-options.png "Opciones para una copia de seguridad.") 

## Creación de una copia de seguridad manual

Además de copias de seguridad planificadas, puede crear una copia de seguridad manualmente. Para crear una copia de seguridad manual, siga los pasos para ver las copias de seguridad existentes y luego pulse **Copia de seguridad ahora** sobre la lista de copias de seguridad disponibles. Se mostrará un mensaje que le indicará que la copia de seguridad se ha iniciado y se añade una copia de seguridad 'pendiente' a la lista de copias de seguridad disponibles.

## Restauración de una copia de seguridad
Para restaurar una copia de seguridad en una nueva instancia de servicio, siga los pasos para ver las copias de seguridad existentes y luego pulse en la fila correspondiente para ampliar las opciones para la copia de seguridad que desea descargar. Pulse el botón **Restaurar**. Se mostrará un mensaje que le indicará que se ha iniciado una restauración. A la nueva instancia del servicio se le asignará automáticamente el nombre "janusgraph-restore-[timestamp]" y aparecerá en el panel de control cuando comience el suministro.
