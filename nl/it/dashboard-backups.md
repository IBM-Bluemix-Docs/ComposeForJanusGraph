---

copyright:
  years: 2017,2018
lastupdated: "2017-10-16"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Backup
{: #backups}

Puoi creare e scaricare i backup dalla scheda _Backups_ della pagina _Manage_ del tuo dashboard del servizio. Sono disponibili backup giornalieri, settimanali, mensili e su richiesta. Vengono conservati in base alla seguente pianificazione:

Tipo di backup|Pianificazione conservazione
----------|-----------
Giornaliero|I backup giornalieri sono conservati per 7 giorni
Settimanale|I backup settimanali sono conservati per 4 settimane
Mensile|I backup mensili sono conservati per 3 mesi
Su richiesta|Il backup su richiesta viene conservato. Il backup conservato è sempre il più recente backup su richiesta.
{: caption="Tabella 1. Pianificazione conservazione backup" caption-side="top"}

Le politiche di conservazione e di pianificazione del backup sono fissate. Se hai bisogno di conservare più backup rispetto a quelli consentiti dalla pianificazione di conservazione, devi scaricare i backup e conservare gli archivi in base ai tuoi requisiti di business.

## Visualizzazione dei backup esistenti

I backup giornalieri del tuo database vengono automaticamente pianificati. Per visualizzare i tuoi backup esistenti:

1. Passa alla pagina _Manage_ del tuo dashboard del servizio.
2. Fai clic su **Backups** nelle schede per aprire la pagina _Backups_. Viene visualizzato un elenco di backup disponibili, con i backup più recenti all'inizio dell'elenco:

  ![Backup disponibili](./images/janusgraph-backups-show.png "Un elenco di backup disponibili, incluso un backup in attesa")

Fai clic sulla riga corrispondente per espandere le opzioni per ogni backup disponibile.
  ![Opzioni di backup](./images/janusgraph-backups-options.png "Opzioni di backup.") 

### Utilizzo dell'API per visualizzare i backup esistenti

Un elenco dei backup è disponibile nell'endpoint `GET /2016-07/deployments/:id/backups`. L'endpoint fondazione, con l'ID istanza di servizio e l'ID distribuzione, sono visualizzati nella _Panoramica_ del servizio. Ad esempio: 
``` 
https://composebroker-dashboard-public.mybluemix.net/api/2016-07/instances/$INSTANCE_ID/deployments/$DEPLOYMENT_ID/backups
```  

## Creazione di un backup manuale

Come per i backup pianificati puoi creare un backup manualmente. Per creare un backup manuale, segui le istruzioni per visualizzare i backup esistenti, quindi fai clic su **Back up now** sopra l'elenco dei backup disponibili. Viene visualizzato un messaggio per farti sapere che è stato avviato un backup ed è stato aggiunto un backup 'in sospeso' all'elenco dei backup disponibili.

### Utilizzo dell'API per creare un backup

Invia una richiesta POST all'endpoint di backup per avviare un backup manuale: `POST /2016-07/deployments/:id/backups`. Restituisce immediatamente l'ID ricetta e le informazioni sul backup mentre è in esecuzione. Dovrai controllare l'endpoint dei backup per vedere se il backup è terminato e trovare il relativo backup_id prima di utilizzarlo. Utilizza `GET /2016-07/deployments/:id/backups/`.

## Ripristino di un backup

Per ripristinare un backup in una nuova istanza, segui le istruzioni per visualizzare i backup esistenti, quindi fai clic sulla riga corrispondente per espandere le opzioni del backup che desideri scaricare. Fai clic sul pulsante **Restore**. Viene visualizzato un messaggio per farti sapere che è stato avviato un ripristino. La nuova istanza del servizio sarà automaticamente denominata "janusgraph-restore-[timestamp]" e visualizzata nel tuo dashboard dopo l'avvio del provisioning.

### Ripristino tramite la CLI {{site.data.keyword.cloud_notm}}

Utilizza la seguente procedura per ripristinare un backup da un servizio JanusGraph in esecuzione a un nuovo servizio JanusGraph utilizzando la CLI {{site.data.keyword.cloud_notm}}. 
1. Se ne hai bisogno, [scaricala e installala](https://console.bluemix.net/docs/cli/index.html#overview). 
2. Trova il backup da cui vuoi eseguire il ripristino nella pagina _Backups_ del tuo servizio e copia il backup_id.  
  **In alternativa**  
  Usa `GET /2016-07/deployments/:id/backups` per trovare un backup e il suo ID tramite la API Compose. L'endpoint fondazione e l'ID istanza di servizio sono entrambi visualizzati nella _Panoramica_ del servizio. Ad esempio: 
  ``` 
  https://composebroker-dashboard-public.mybluemix.net/api/2016-07/instances/$INSTANCE_ID/deployments/$DEPLOYMENT_ID/backups
  ```  
  La risposta avrà un elenco di tutti i backup disponibili per tale istanza del servizio. Scegli il backup da cui vuoi eseguire il ripristino e copiane l'ID.

3. Accedi con l'account e le credenziali appropriati. `bx login` (oppure `bx login -help` per visualizzare tutte le opzioni di login).

4. Passa alla tua organizzazione e al tuo spazio: `bx target -o "$YOUR_ORG" -s "YOUR_SPACE"`

5. Usa il comando `service create` per eseguire il provisioning di un nuovo servizio e fornire il servizio di origine e lo specifico backup che stai ripristinando in un oggetto JSON. Ad esempio:
``` 
bx service create SERVICE PLAN SERVICE_INSTANCE_NAME -c '{"source_service_instance_id": "$SERVICE_INSTANCE_ID", "backup_id": "$BACKUP_ID" }'
```
  Il campo _SERVICE_ deve essere compose-for-janusgraph e il campo _PLAN_ deve essere Standard o Enterprise, a seconda del tuo ambiente. _SERVICE\_INSTANCE\_NAME_ è dove inserisci il nome per il tuo nuovo servizio. _source\_service\_instance\_id_ è l'ID dell'istanza di servizio dell'origine del backup; può essere ottenuto eseguendo `bx cf service DISPLAY_NAME --guid`, dove _DISPLAY\_NAME_ è il nome del servizio JanusGraph da cui proviene il backup. 
  
  Gli utenti Enterprise dovranno anche specificare nell'oggetto JSON il cluster nel quale eseguire la distribuzione con il parametro `"cluster_id": "$CLUSTER_ID"`.
  
### Migrazione a una nuova versione

Alcuni upgrade di versione principale non sono disponibili nella distribuzione in esecuzione attuale. Dovrai eseguire il provisioning di un nuovo servizio che sta eseguendo la versione di cui è stato eseguito l'upgrade e migrare quindi in essa i tuoi dati utilizzando un backup. Questo processo è lo stesso di un ripristino di un backup di cui sopra, con l'unica differenza che specificherai la versione alla quale desideri eseguire l'upgrade.

``` 
bx service create SERVICE PLAN SERVICE_INSTANCE_NAME -c '{"source_service_instance_id": "$SERVICE_INSTANCE_ID", "backup_id": ""$BACKUP_ID", "db_version":"$VERSION_NUMBER" }'
```

Ad esempio, il ripristino di una versione meno recente di un servizio {{site.data.keyword.composeForJanusGraph}} a un nuovo servizio che esegue JanusGraph 0.1.1 sarà simile a:
```
bx service create compose-for-janusgraph Standard migrated_janusgraph -c '{ "source_service_instance_id": "0269e284-dcac-4618-89a7-f79e3f1cea6a", "backup_id":"5a96d8a7e16c090018884566", "db_version":"0.1.1"  }'
