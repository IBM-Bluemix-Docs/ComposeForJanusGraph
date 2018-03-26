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

# Connessione a JanusGraph

JanusGraph supporta le connessioni utilizzando le richieste HTTP o tramite WebSockets. Tutte le connessioni sono eseguite tramite HTTPS e richiedono l'autenticazione. Le richieste basate su HTTP supportano l'autenticazione token o di base. Per altri tipi di richieste diverse da una chiamata una tantum nella distribuzione JanusGraph, utilizzando WebSockets o il token di autenticazione si ottengono prestazioni migliori. L'autenticazione di base è un processo costoso che rallenta le richieste.

Per ulteriori informazioni su come utilizzare HTTPs e sulla creazione e apertura dei grafici, consulta [Creazione e trasmissione di un grafico utilizzando HTTPS](./tutorial-https.html).
{: .tip}

La comunicazione corrente tra il client e il database implica che il client invia le richieste in Gremlin, un dialetto del linguaggio Groovy personalizzato per l'esecuzione di query per le strutture del grafico e del database che risponde a queste query. Puoi anche inviare le richieste utilizzando la console Gremlin.

Per i dettagli su come ottenere, installare, configurare e utilizzare la console Gremlin, consulta [Creazione e trasmissione di un grafico utilizzando la console Gremlin](./tutorial-gremlin-console.html).
{: .tip}

## Richieste HTTP

Se stai utilizzando le richieste HTTP per collegarti al server, può essere utilizzato un solo endpoint per comunicare con JanusGraph configurato come round robin tra tutti i nodi disponili nella distribuzione. Include la porta e il nome host.

Puoi ottenere le tue stringhe di connessione HTTPS dalla tua pagina della panoramica del dashboard del servizio. La stringa di connessione richiede l'autenticazione di base utilizzando le credenziali da utente amministratore per il collegamento al server.

Per collegarti a {{site.data.keyword.composeForJanusGraph}} con una richiesta HTTP devi utilizzare il metodo HTTP POST con l'endpoint e inviare un documento formattato JSON con una sola voce con la chiave "gremlin" e un valore della query Gremlin che desideri inviare. 

```json
{
    "gremlin": "a gremlin query would go here"
}
```

Gremlin è un linguaggio trasversale estensivo ma un programma in esso può anche essere semplice tanto quanto "1+1". Per passarlo come uno script in un comando `curl` la parte di dati di una richiesta POST è:

```
'{"gremlin" : "1+1" }'
``` 

Se stai inviando lo stesso script al server più volte, puoi migliorare le prestazioni fornendo un dizionario "bindings" facoltativo con una mappa di variabili disponibili per lo script gremlin. Questo migliora le prestazioni perché il server non deve ricompilare lo script gremlin.
{: .tip}

Ulteriori informazioni sul protocollo HTTP possono essere trovate nella sezione [Connecting via REST](http://tinkerpop.apache.org/docs/3.2.3/reference/#_connecting_via_rest) della documentazione Apache Tinkerpop

Per inviare uno script Gremlin all'endpoint, devi essere autenticato. Esistono due modi per essere autenticato con l'accesso alla richiesta HTTP: l'autenticazione di base e tramite token.

## Autenticazione di base

L'autenticazione di base invia i tuoi nome utente e password con la richiesta. Per una query una tantum, è ragionevole, ma non più di questo e inizia a caricare il server con la complessità di scambiare e convalidare la password. Con curl puoi passare il nome utente e la password con un'opzione `-u`:

```shell
$ curl -XPOST -d '{"gremlin" : "1+1" }' "https://portal91-0.ianuspater.compose-3.composedb.com:17916" -u admin:PASSWORDHERE
```

Puoi anche integrare il nome utente e la password nell'URL endpoint. 

## Autenticazione token

Per richieste HTTP più efficienti, puoi ottenere un token di sessione da un altro endpoint. Questo token viene passato nell'intestazione di tutte le successive richieste. Se stai effettuando più di una chiamata nell'endpoint HTTP, puoi migliorare le prestazioni utilizzando l'autenticazione tramite token.

Utilizza le stringhe di connessione nel pannello _Session_ della pagina della panoramica del dashboard del servizio. Le stringhe di connessione già includono la chiamata necessaria per ottenere il token come un comando curl:

```shell
$ curl -XGET "https://admin:PASSWORDHERE@portal91-0.ianuspater.compose-3.composedb.com:17916/session"
```

Il risultato JSON restituito contiene il token di sessione:

```
{
  "token": "YWRtaW46MTQ5NjkyODM1NDgzMzpWYXlxMERsMWs0VmltdGo3WUpzMEwtN3prUi00TGxZR3J6LXZnbDVmN3lnPQ=="
}
```

Il token è valido per 60 minuti
{: .tip}

Estrai il valore del token e includilo nell'intestazione `Authorization` in tutte le successive richieste:

```shell
$ curl -XPOST -H "Authorization: Token YWRtaW46MTQ5NjkyODM1NDgzMzpWYXlxMERsMWs0VmltdGo3WUpzMEwtN3prUi00TGxZR3J6LXZnbDVmN3lnPQ==" -d '{"gremlin" : "1+1" }' "https://portal91-0.ianuspater.compose-3.composedb.com:17916"
```

Se stai facendo ciò dalla shell e disponi del comando [`jq`](https://stedolan.github.io/jq/), puoi configurare una variabile di ambiente che esegue un comando simile al seguente:

```shell
$ export JGTOKEN=`curl -s -XGET "https://admin:PASSWORDHERE@portal91-0.ianuspater.compose-3.composedb.com:17916/session" | jq -r .token`
```

E quindi eseguire le richieste nel seguente modo:

```shell
$ curl -XPOST -H "Authorization: Token $JGTOKEN" -d '{"gremlin" : "1+1" }' "https://portal91-0.ianuspater.compose-3.composedb.com:17916"
```

## Risposte JSON HTTP JSON

Le risposte alle seguenti query sono nel formato di un documento JSON con un valore ID di richiesta, un oggetto di stato e un oggetto risultato. Questo è un esempio di una risposta:

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
Il `requestId` è un identificativo univoco per la richiesta. `status` contiene un messaggio leggibile, un codice di stile di stato HTTP e altri attributi dello stato riportato. `result` contiene un oggetto di dati strutturato in base a quello che il codice Gremlin eseguito restituisce e una sezione di metadati per ogni ulteriore informazione al di fuori dello scopo dei dati del risultato.

## Websockets

WebSockets è un modo efficiente per creare una connessione a due vie duratura su TCP e TLS che è ideale per le sessioni interattive tra le applicazioni client e il server JanusGraph. Varie librerie di JanusGraph utilizzano WebSockets come loro connessione al server; per utilizzare JanusGraph in Compose, devono essere in grado di eseguire l'autenticazione di base e di utilizzare WSS, WebSockets sicuro in TLS. 

Puoi ottenere le tue stringhe di connessione Websockets dalla sezione _Websocket_ della tua pagina della panoramica del dashboard del servizio. Gli URI possono essere utilizzati per stabilire una sessione a esecuzione prolungata con la distribuzione JanusGraph e hanno il prefisso di `wss:` per indicare che le connessioni saranno protette da HTTPS. Queste stringhe di connessione richiedono l'autenticazione di base per essere utilizzate con le credenziali da utente amministratore per il collegamento al server.

## Console Gremlin

La console Gremlin è lo strumento essenziale per utilizzare i server abilitati Tinkerpop. La console Gremlin utilizza la connettività WebSockets, ma non utilizza la sintassi URI per la connessione; questo in parte perché ha anche bisogno di configurare altri elementi per utilizzare la connessione.

Per configurare la console Gremlin fornisci un file YAML con i dettagli appropriati. Puoi trovare questa configurazione nel pannello _Gremlin Console YAML_ nella pagina della panoramica del dashboard del servizio.

Per i dettagli su come ottenere, installare, configurare e utilizzare la console Gremlin, consulta [Creazione e trasmissione di un grafico utilizzando la console Gremlin](./tutorial-gremlin-console.html).
