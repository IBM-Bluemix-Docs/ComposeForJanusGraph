---

Copyright:
  Years: 2017
lastupdated: "2017-09-05"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Panoramica sul servizio

La pagina _Overview_ ti mostra le informazioni sul tuo database {{site.data.keyword.cloud}} Compose. La panoramica include le informazioni di identificazione essenziali e l'utilizzo della risorsa corrente. Troverai inoltre una sezione per le stringhe di connessione che puoi utilizzare con gli strumenti o utilizzare gli strumenti per il collegamento al tuo database.

## Dettagli della distribuzione

Il pannello _Deployment Details_ mostra i dettagli del tuo servizio.

![Dettagli della distribuzione](./images/janusgraph-deployment-details.png "Una vista del pannello dei dettagli della distribuzione")

### Tipo

Il tipo di database offerto dal servizio e la versione che il servizio utilizza.

### Nome

Un identificativo interno per il servizio.

### Utilizzo

La dimensione del tuo database e la quantità di memoria fornita dal tuo piano di servizio.


## Stringhe di connessione

Le stringhe di connessione possono essere utilizzate da alcune librerie client e contenere tutte le informazioni necessarie al collegamento di altre librerie. Puoi trovare come utilizzare una stringa di connessione al tuo servizio in [Connessione a un'applicazione esterna](./connecting-external.html).

Troverai ogni stringa di connessione per il tuo servizio in una scheda differente nel pannello _Connection Strings_.

### Sessione 

Puoi utilizzare una sessione URI per ottenere un token valido per 60 minuti. Dovresti utilizzare il token fornito quando effettui le chiamate alla distribuzione utilizzando gli URI HTTPS o Websocket.

### HTTPS

Questa è la stringa di connessione fondamentale per una distribuzione JanusGraph. Per utilizzare una stringa di connessione HTTPS devi fornire le credenziali da utente amministratore per il collegamento al server. Per i dettagli su come utilizzare HTTPs, consulta [Creazione e trasmissione di un grafico utilizzando HTTPS](./tutorial-https.html).

### Websocket

Gli URI Websocket possono essere utilizzati per stabilire una sessione a esecuzione prolungata con la distribuzione JanusGraph. Hanno il prefisso di `wss:` per indicare che le connessioni saranno protette da HTTPS. Queste stringhe di connessione richiedono l'autenticazione di base per essere utilizzate con le credenziali da utente amministratore per il collegamento al server.

Esistono varie librerie di JanusGraph che utilizzano WebSockets come loro connessione al server; per utilizzare {{site.data.keyword.composeForJanusGraph}} devono essere in grado di eseguire l'autenticazione di base e di utilizzare WSS (WebSockets sicuro in TLS).

### YAML console Gremlin

Puoi utilizzare entrambe le configurazioni fornite per collegarti a {{site.data.keyword.composeForJanusGraph}} utilizzando la console Gremlin. Per i dettagli su come utilizzare il file YAML della console Gremlin, consulta [Creazione e trasmissione di un grafico utilizzando la console Gremlin](./tutorial-gremlin-console.html).
