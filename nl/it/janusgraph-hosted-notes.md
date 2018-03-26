---
copyright:
  years: '2015, 2017'
lastupdated: '2017-11-21'
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Note ospitate su JanusGraph

{{site.data.keyword.composeForJanusGraph_full}} è un servizio ospitato, per cui assicurare che le distribuzioni siano protette differisce in alcuni casi dall'esecuzione di JanusGraph in un server dedicato. Questo argomento evidenzia queste differenze e variazioni. La maggior parte di queste modifiche influenza come funzionano le query del linguaggio Gremlin.

## Sandbox

Per garantire che le query Gremlin non accedano a funzionalità che potrebbero compromettere il sistema, tutte le query vengono eseguite in un sandbox. Questo significa che tutte le chiamate metodo e funzione devono avere una firma che corrisponde a una delle seguenti espressioni regolari:

```
java\.util.*
java\.lang\.Object.*
java\.lang\.Class <T extends java\.lang\.Object>#getSimpleName\(
java\.lang\.Iterable <T extends java\.lang\.Object>#iterator\(\)
java\.lang\.Boolean(?!#getBoolean\().*
java\.lang\.Double.*
java\.lang\.Float.*
java\.lang\.Integer(?!#getInteger\().*
java\.lang\.Long(?!#getLong\().*
java\.lang\.Math.*
java\.lang\.String(?!#intern\(\)).*
java\.lang\.StringBuilder.*
java\.lang\.Object#equals\(
java\.lang\.Object#hashCode\(
java\.util\.ArrayList#equals\(

org\.codehaus\.groovy\.runtime\.DefaultGroovyMethods.*
org\.codehaus\.groovy\.runtime\.StringGroovyMethods.*
org\.apache\.commons\.configuration\.MapConfiguration.*
org\.apache\.tinkerpop\.gremlin\.structure(?!\.io|\.Graph#toString\(|\.Graph#variables\(|\.Graph#configuration\(|\.Graph#io\(\)|\.util\.GraphFactory.*).*
org\.apache\.tinkerpop\.gremlin\.process\.traversal.*
org\.apache\.tinkerpop\.gremlin\.util\.Gremlin#version\(\)
org\.janusgraph\.core(?!\.JanusGraphFactory.*).*
org\.janusgraph\.graphdb(?!\.tinkerpop\.JanusGraphBlueprintsGraph#toString\(|\.tinkerpop\.JanusGraphBlueprintsGraph#variables\(|\.tinkerpop\.JanusGraphBlueprintsGraph#configuration\(|\.tinkerpop\.JanusGraphBlueprintsGraph#io\().*
org\.janusgraph\.example\.GraphOfTheGodsFactory(?!#create\(|#main\().*
org\.apache\.tinkerpop\.gremlin\.groovy\.plugin\.dsl\.credential\.CredentialGraph.*

Script\d+#.+\(?.+\)
groovy\.lang\.Closure <V extends java\.lang\.Object>#call\(?.+\)

org\.janusgraph\.util\.stats\.MetricManager#getRegistry\(\)
org\.janusgraph\.util\.stats\.MetricManager#INSTANCE
org\.apache\.tinkerpop\.gremlin\.util\.MetricManager#getRegistry\(\)
org\.apache\.tinkerpop\.gremlin\.util\.MetricManager#INSTANCE
```

Il tentare di eseguire una chiamata metodo o funzione che non corrisponde ad alcuna delle voci nella whitelist delle funzioni genera un errore: 

```
[Static type checking] - Not authorized to call this method: ...
```

Il sandbox richiede inoltre che tutte le variabili vengano dichiarate staticamente per abilitare il controllo di sicurezza. Puoi farlo con il comando `def`.

## Sessioni

Una sessione consente una connessione a {{site.data.keyword.composeForJanusGraph}} per conservare lo stato tra le richieste. La maggior parte delle opzioni di connessione a {{site.data.keyword.composeForJanusGraph}} non hanno sessioni associate ad esse. Questo significa che tutti gli script Gremlin inviati tramite questi metodi di connessione hanno bisogno di essere completamente autonomi. Ad esempio, aprendo un qualsiasi grafico con cui lo script deve funzionare, la query dei nodi appropriati e l'attraversamento del grafico di questi nodi dovranno tutti essere gestiti in una richiesta. Questa limitazione si applica sia richieste HTTP che alle connessioni WebSockets. 

L'eccezione a questo è quando effettui il collegamento dalla console Gremlin utilizzando `:remote connect`.

```
:remote connect tinkerpop.server conf/compose.yaml
```

Se effettui il collegamento utilizzando il precedente comando non c'è alcuna sessione e la connessione funziona nello stesso modo delle richieste HTTP. Ma se aggiungi `session` come un argomento, il supporto della sessione è ora abilitato.

```
:remote connect tinkerpop.server conf/compose.yaml session
```

Utilizzando una sessione, puoi definire una variabile in un comando e fare riferimento a essa in un altro comando.
{: .tip}

## Transazioni

Tutte le modifiche al database JanusGraph sottostante sono incapsulate in una transazione. Le transazioni consentono il commit di tutte le modifiche al loro interno per renderle permanenti o di eseguirne il rollback per assicurarsi che non lo siano. 

Quando effettui una richiesta tramite una richiesta HTTP o un WebSocket, viene automaticamente avviata una transazione quando effettui una modifica che deve essere scritta nel database. Viene inoltre eseguito automaticamente il commit della transazione quando la richiesta è completa.

L'eccezione è quando stai eseguendo il collegamento utilizzando la console Gremlin con la sessione abilitata. La sessione può essere di lunga durata, quindi spetta all'utente il commit di tutte le modifiche richiamando `graph.tx().commit()`, dove `graph` è il grafico aperto che contiene le modifiche. Questo significa inoltre che puoi richiamare `graph.tx().rollback()` per riportare indietro lo stato del grafico a prima dell'inizio della transazione. 

## Indici misti

La versione corrente di {{site.data.keyword.composeForJanusGraph}} non supporta gli indici misti.
