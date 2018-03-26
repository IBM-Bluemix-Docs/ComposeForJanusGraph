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

# Creazione e trasmissione di un grafico utilizzando la console Gremlin

Questo argomento ti aiuterà ad iniziare ad utilizzare il tuo primo database {{site.data.keyword.composeForJanusGraph_full}}. In questa esercitazione stiamo per utilizzare la console Gremlin per inviare i comandi al server JanusGraph.

## 1. Installa Gremlin

Per installare la console Gremlin:

1. Assicurati di avere una versione recente di Java installata.
2. Scarica [Gremlin Console version 3.2.3](https://archive.apache.org/dist/tinkerpop/3.2.3/apache-tinkerpop-gremlin-console-3.2.3-bin.zip).
3. Decomprimi il file scaricato in una directory di lavoro.
4. Utilizzando la riga di comando o il terminale, modifica la directory con la directory root della console Gremlin e verifica lo scaricamento eseguendo `bin/gremlin.sh`:

  ```text
  $ unzip -q  ~/Downloads/apache-tinkerpop-gremlin-console-3.2.3-bin.zip
  $ cd apache-tinkerpop-gremlin-console-3.2.3
  $ bin/gremlin.sh

          \,,,/
          (o o)
  -----oOOo-(3)-oOOo-----
  plugin activated: tinkerpop.server
  plugin activated: tinkerpop.utilities
  plugin activated: tinkerpop.tinkergraph
  gremlin> [CONTROL-D]                                                             $

  ```

5. Configura la console Gremlin. Puoi trovare il file YAML della tua console Gremlin nella pagina *Overview* del tuo servizio {{site.data.keyword.composeForJanusGraph}}. Salva una delle configurazioni in un file nella directory `conf`. In questo esempio, la salveremo come `conf/compose.yaml`.
 
## 2. Collegati a Compose for JanusGraph

Per verificare la connessione, esegui nuovamente `bin/gremlin.sh`. Quindi, utilizza il comando `:remote` per collegarti al server tinkerpop:

```text
:remote connect tinkerpop.server conf/compose.yaml session
```

Questo comando comunica a Gremlin di `connect` a qualcosa di simile a un `tinkerpop.server` utilizzando la configurazione in `conf/compose.yaml`. Stiamo anche trasmettendo un altro argomento - `session` - per abilitare il supporto della sessione.

Immetti `:help :remote` per visualizzare le opzioni del comando `:remote`.
{: .tip}

Se la connessione ha esito positivo visualizzerai un'avvertenza SSL e un messaggio per confermare la connessione.

```text
WARN  org.apache.tinkerpop.gremlin.driver.Cluster  - SSL configured without a trustCertChainFile and thus trusts all certificates without verification (not suitable for production)
==>Configured portal93-4.ianuspater.compose-3.composedb.com/159.8.153.216:18045-[2378d0de-0a93-4330-b8d8-848667f5b117]
gremlin>
```

## 3. Invia i comandi al server.

La console Gremlin non inoltra nulla al server remoto automaticamente: per inviare un comando al server hai bisogno di aggiungere al comando il prefisso `:>`. In alternativa puoi reindirizzare tutti i comandi della console al server remoto con `:remote console`. Questo viene illustrato dalle seguenti serie di comandi e risposte:

```text
$ bin/gremlin.sh                                                                   

        \,,,/
        (o o)
-----oOOo-(3)-oOOo-----
plugin activated: tinkerpop.server
plugin activated: tinkerpop.utilities
plugin activated: tinkerpop.tinkergraph
gremlin> 1+1
==>2
gremlin> :> 1+1
==>No remotes are configured.  Use :remote command.
gremlin> :remote connect tinkerpop.server conf/compose.yaml session
WARN  org.apache.tinkerpop.gremlin.driver.Cluster  - SSL configured without a trustCertChainFile and thus trusts all certificates without verification (not suitable for production)
==>Configured portal93-4.ianuspater.compose-3.composedb.com/159.8.153.216:18045-[016d7a68-cf70-450e-92eb-4e5e2a647b5b]
gremlin> :remote console
==>All scripts will now be sent to Gremlin Server - [portal93-4.ianuspater.compose-3.composedb.com/159.8.153.216:18045]-[016d7a68-cf70-450e-92eb-4e5e2a647b5b] - type ':remote console' to return to local mode
gremlin> 1+1
==>2
gremlin> 

```

## 4. Crea il grafico

{{site.data.keyword.composeForJanusGraph}} ha un factory grafico dedicato per la creazione, l'apertura e la chiusura dei grafici. L'utilizzo di questo factory - `ConfiguredGraphFactory` - significa che non devi conoscere il meccanismo di archiviazione sottostante, per cui la creazione di un nuovo grafico è solo una questione di denominazione. Inizia creando un nuovo grafico. Nei comandi di esempio in questa esercitazione, stiamo chiamando il nostro grafico _mygraph_.

Utilizza la parola chiave `def` per dichiarare la variabile graph.

```
def graph=ConfiguredGraphFactory.create("mygraph")
```

I nomi del grafico {{site.data.keyword.composeForJanusGraph}} possono includere solo caratteri alfanumerici e di sottolineatura.
{: .tip}

## 5. Apri il grafico

Dopo aver creato un grafico, per utilizzarlo, devi aprirlo. Puoi farlo richiamando `open()` in `ConfiguredGraphFactory`. Apriamo il nostro grafico:

```
def graph=ConfiguredGraphFactory.open("mygraph")
```

A questo punto dobbiamo eseguire il commit dell'operazione in modo che il grafico persista:

```
graph.tx().commit()
```

Se chiudiamo la nostra sessione ora, il nuovo grafico sarà qui quando torniamo.

## 6. Aggiungi i vertici al grafico

Il nostro grafico conterrà le informazioni sugli sviluppatori software e le parti di software che hanno sviluppato. Aggiungiamo una persona denominata "marko" e una parte di software chiamata "lop".

Per prima cosa, aggiungiamo marko.

```
def v1 = graph.addVertex(T.label, "person", "name", "marko", "age", 29)
```

Se corretto, questo comando restituirà l'indice del vertice che abbiamo aggiunto - `==>v[4304]` - e possiamo continuare aggiungendo il secondo vertice.

```
def v2 = graph.addVertex(T.label, "software", "name", "lop", "lang", "java")
```

Ancora una volta, dobbiamo eseguire il commit delle operazioni.

```
graph.tx().commit()
```

## 7. Trova un vertice nel grafico

Per trovare un vertice, quello con il nome "marko", dobbiamo prima attraversare il grafico e quindi utilizzare il metodo has() che restituisce l'identificativo del vertice.

```
def g=graph.traversal()
g.V().has("name","marko")
```

Per riassegnare i nostri vertici alle variabili v1 e v2, sapere se abbiamo chiuso la nostra vecchia sessione e avviatone una nuova, utilizziamo next().

```
def v1 = g.V().has("name","marko").next()
def v2 = g.V().has("name","lop").next()
```

Possiamo quindi utilizzare queste variabili nei successivi comandi Gremlin.

## 8. Aggiungi un bordo

Successivamente possiamo aggiungere un bordo pesato tra due vertici, fornendo al bordo un'etichetta "created" e un peso di 0.4.

Tieni presente che devi trasmettere il numero come un decimale, specificando il valore come `0.4d`.

```
v1.addEdge("created", v2, "weight", 0.4d)
```

Nuovamente, assicurati di eseguire il commit della modifica.

## 9. Esegui la query del grafico

Puoi ora eseguire la query del tuo grafico per trovare il nome di tutto il software creato da Marko.

```
g.V().has("name","marko").out("created").values("name")
```
