---

Copyright:
  Years: 2017
lastupdated: "2017-10-24"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Introduzione a Compose for JanusGraph
{: #getting-started-with-compose-for-janusgraph}

{{site.data.keyword.composeForJanusGraph_full}} è un database grafico scalabile ottimizzato per l'archiviazione e l'esecuzione di query di dati altamente interconnessi modellato da milioni o miliardi di vertici e bordi. Il richiamo di dati efficiente e semplice da queste strutture complesse è abilitato dalla compatibilità Apache Tinkerpop(TM) di JanusGraph, che ti consente di eseguire query efficienti che sarebbero difficili o impossibili con un database relazionale tradizionale. {{site.data.keyword.composeForJanusGraph}} rende JanusGraph sempre migliore gestendolo al tuo posto al tuo posto, offrendo un sistema di distribuzione scalabile facile da utilizzare che distribuisce l'elevata disponibilità e la ridondanza, i backup continui automatizzati e altro ancora.
{:shortdesc}

## Creazione di un'istanza del servizio Compose per JanusGraph

[Crea un'istanza {{site.data.keyword.composeForJanusGraph}}](https://console.bluemix.net/catalog/services/compose-for-janusgraph/).

Quando crei un'istanza del servizio, assicurati di scegliere un nome per il tuo servizio e un nome per la credenziale. Lascia il servizio senza bind; puoi collegare un'applicazione al tuo servizio successivamente utilizzando le credenziali fornite quando è stato eseguito il provisioning del servizio. I vari valori delle credenziali e come utilizzarli in un'applicazione sono dettagliati in [Connessione a un'applicazione {{site.data.keyword.cloud}}](./connecting-bluemix-app.html).

Quando esegui il provisioning della tua istanza, puoi scegliere i piani *Standard* o *Enterprise*. Con il piano *Enterprise*, puoi eseguire il provisioning della tua istanza in un cluster {{site.data.keyword.composeEnterprise}} disponibile. {{site.data.keyword.composeEnterprise}} fornisce la sicurezza e l'isolamento necessari per la conformità aziendale e utilizza la rete dedicata per garantire le prestazioni dei database distribuiti. Per ulteriori dettagli, consulta la [Documentazione aziendale Compose](../ComposeEnterprise/index.html).

## Gestione di Compose for JanusGraph

Puoi gestire il tuo servizio dal dashboard del servizio. Qui puoi trovare le informazioni sul tuo database {{site.data.keyword.cloud_notm}} Compose e su come collegarti ad esso. Puoi anche:
- gestire i tuoi backup
- assegnare ulteriori risorse al tuo servizio
- modificare la password del servizio
- utilizzare le whitelist per limitare l'accesso ai tuoi database 
Per ulteriori informazioni, consulta [Impostazioni](./dashboard-settings.html).

## Connessione a Compose for JanusGraph

Puoi collegarti al tuo servizio con le credenziali create insieme al servizio o con le stringhe di connessione e la riga di comando forniti nella scheda *Overview* nel tuo dashboard del servizio. 

Se desideri collegarti al tuo servizio dall'esterno di {{site.data.keyword.cloud_notm}}, puoi utilizzare la riga di comando o le stringhe di connessione fornite. Puoi trovare ulteriori informazioni in [Connessione a JanusGraph](./connecting-external.html).

## Connessione a un'applicazione {{site.data.keyword.cloud_notm}}

Per collegare un'applicazione {{site.data.keyword.cloud_notm}} al tuo servizio, utilizza le credenziali create insieme al servizio. Puoi trovare le informazioni su come collegare un'applicazione {{site.data.keyword.cloud_notm}} a un servizio {{site.data.keyword.composeForJanusGraph}} in [Connessione a un'applicazione {{site.data.keyword.cloud_notm}}](./connecting-bluemix-app.html).

## Utilizzo del browser dati JanusGraph 

Esplorare i tuoi dati del grafico dalla riga di comando può essere un'attività complessa. Il browser dati per {{site.data.keyword.composeForJanusGraph}} combina un modo facile di utilizzare il builder di query con le schede della risposta della query avanzate che visualizza i risultati delle tue query come una vista JSON interattiva e come un grafico visualizzato. Per ulteriori informazioni, leggi [Utilizzo del browser dati JanusGraph](./data-browser.html)
