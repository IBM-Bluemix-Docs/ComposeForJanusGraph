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

# Connessione a un'applicazione {{site.data.keyword.cloud_notm}}

Per collegare un'applicazione {{site.data.keyword.cloud}} al tuo servizio, utilizza le credenziali create quando è stato eseguito il provisioning del servizio.

## Credenziali disponibili

Nome campo|Descrizione
----------|-----------
`deployment_id`|Un identificativo interno per il servizio come creato in Compose.
`name`|Il nome di distribuzione del database.
`db_type`|Il tipo di database offerto dal servizio; in questo caso `JanusGraph`.
`uri_cli`|Una riga di comando `CURL` che illustra come inviare una query Gremlin con un'autenticazione basata sul token.
`uri_cli_1`|Una riga di comando `CURL` secondaria che si collega all'istanza del database.
`maps`|non utilizzato
`uri`|L'URI `https` utilizzato quando esegui il collegamento al servizio, che include il nome host del server e il numero di porta a cui collegarsi. Consulta [Connessione di un'applicazione esterna](./connecting-external.html).
`uri_direct_1`|Un URI `https` secondario per il collegamento al servizio.
`misc`|Un campo principale contenente i campi `session`, `websocket` e `gremlin_console_yaml`.
`misc.session`| Un URI `https` che viene utilizzato per richiamare un token di autenticazione di 60 minuti che può essere utilizzato per interagire con il servizio. Consulta [Connessione di un'applicazione esterna - Token di autenticazione](./connecting-external.html#token-authentication).
`misc.websocket`|Gli URI `wss` utilizzati durante il collegamento al servizio utilizzando Websockets. Consulta [Connessione di un'applicazione esterna - Websockets](./connecting-external.html#websockets).
`misc.gremlin_console_yaml`|Le informazioni YAML utilizzate per configurare la console Gremlin per il collegamento al servizio.  Consulta [Connessione di un'applicazione esterna - Console Gremlin](./connecting-external.html#gremlin-console).
{: caption="Tabella 1. Credenziali di Compose for JanusGraph" caption-side="top"}

## Utilizzo delle credenziali

La tua applicazione deve essere collegata al tuo servizio per utilizzare le credenziali del servizio. La seguente procedura ti mostra come farlo, utilizzando Node.js come esempio.

### Connessione dell'applicazione al servizio

Se già disponi di un'applicazione {{site.data.keyword.cloud_notm}}, puoi collegarla a un servizio dal tuo account {{site.data.keyword.cloud_notm}}:

1. Accedi al tuo account {{site.data.keyword.cloud_notm}}
2. Dal tuo dashboard, seleziona l'applicazione che vuoi collegare al tuo servizio.
3. Nel pannello delle connessioni della pagina _Overview_ dell'applicazione, fai clic su **Connect new** o **Connect existing** per collegarla a un servizio nuovo o esistente di {{site.data.keyword.cloud_notm}}.

  - Se hai fatto clic su **Connect existing**, viene visualizzato un elenco di servizi disponibili. Seleziona il servizio a cui desideri collegare la tua applicazione e fai clic su **Connect**.
  - Se hai fatto clic su **Connect new**, segui le istruzioni per eseguire il provisioning di una nuova istanza del servizio. Quando selezioni un servizio dal catalogo per il provisioning, il campo _Connect to_ sarà automaticamente completato con il nome della tua applicazione.

Se hai un'applicazione che non hai ancora caricato in {{site.data.keyword.cloud_notm}} puoi associarla al servizio prima di caricarlo in {{site.data.keyword.cloud_notm}}: 

1. Accedi al tuo account {{site.data.keyword.cloud_notm}}
2. Scarica e installa lo strumento [Cloud Foundry CLI](https://github.com/cloudfoundry/cli)
3. Collegati a {{site.data.keyword.cloud_notm}} nello strumento della riga di comando e segui le istruzioni per accedere.

  ```
  $ cf api https://api.ng.bluemix.net
  $ cf login
  ```

4. Apri il file `manifest.yml` della tua applicazione.

  - Modifica il valore `host` con qualcosa di univoco. L'host che scegli determinerà il dominio secondario dell'URL della tua applicazione:  `<host>.mybluemix.net`.
  - Modifica il valore `name`. Il valore che scegli sarà il nome dell'applicazione come viene visualizzato nel tuo dashboard {{site.data.keyword.cloud_notm}}.

5. Aggiorna il valore `services` in `manifest.yml` in modo che corrisponda al nome del tuo servizio. `manifest.yml` sarà ora simile a quanto segue:

  ```
  ---
  applications:
  - name:    compose-janusgraph-sample-service
    host:    compose-janusgraph-sample-service
    memory:  256M
    services:
      - compose-janusgraph-sample-app
  ```

### Accesso e utilizzo delle tue credenziali

La tua applicazione deve analizzare le variabili di ambiente Cloud Foundry. In Node.js puoi farlo con il pacchetto [cfenv](https://www.npmjs.com/package/cfenv).

1. Installa il pacchetto con `--save` per aggiornare `package.json`:

  ```
  npm install --save cfenv
  ```

2. Estrai le credenziali {{site.data.keyword.composeForJanusGraph_full}} dalle variabili di ambiente Cloud Foundry.

  ```javascript
  const cfenv = require('cfenv');
  const appenv = cfenv.getAppEnv();

  // Nell'ambiente dell'applicazione (appenv) è presente un oggetto services
  const services = appenv.services;

  // L'oggetto services è un'associazione denominata dal servizio in modo da estrarre il valore per JanusGraph
  let janusgraph_services = services["compose-for-janusgraph"];

  // Ora prendiamo il primo servizio JanusGraph associato e ne estraiamo l'oggetto credentials
  let credentials = janusgraph_services[0].credentials;
  ```

  Puoi ora utilizzare le credenziali del servizio. Ad esempio, per richiamare un URI della sessione che puoi utilizzare per ottenere un token di autenticazione:

  ```javascript
  let jgurl = credentials.misc.session[0];
  ```

3. Trasmetti la tua applicazione a {{site.data.keyword.cloud_notm}}. Quando ne esegui il push, l'applicazione sarà automaticamente associata al servizio.

  ```
  $ cf push
  ```

Per ulteriori informazioni sull'utilizzo delle applicazioni con i servizi {{site.data.keyword.cloud_notm}}, consulta [Aggiunta di un servizio alla tua applicazione](https://console.{DomainName}/docs/services/reqnsi.html#add_service).
