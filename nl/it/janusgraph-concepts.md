---

copyright:
  years: 2017,2018
lastupdated: "2017-12-12"
---

# Concetti su JanusGraph

JanusGraph è un database grafico. Puoi creare, eseguire query e modificare molti grafici nel database. JanusGraph è stato creato basandosi sullo stack Apache Tinkerpop e utilizza il linguaggio Gremlin per l'attraversamento, i comandi e le query.

Per ulteriori informazioni su JanusGraph, consulta [JanusGraph Documentation](http://docs.janusgraph.org/latest/index.html).

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="//www.youtube.com/embed/zTaoMWv6lnE?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Puoi trovare ulteriori video su {{site.data.keyword.composeForJanusGraph}} in [IBM Compose for JanusGraph Learning Center](http://ibm.biz/janusgraph-learning).
{: .tip}

## Introduzione ai database Graph

Fondamentalmente, un grafico è una serie di vertici con i bordi tra di loro. Il vertice, la forma singolare dei vertici, è l'unità fondamentale del grafico e rappresenta alcuni oggetti indivisibili. In termini pratici, può essere una persona, un'ubicazione o un oggetto.  Può avere proprietà per descrivere l'oggetto. 

Un bordo è una connessione tra due vertici espressa con una relazione tra essi. Può avere molteplicità, direzione e proprietà.

Nei database non grafici, le relazioni tra le entità sono una funzione secondaria del database, espressa tramite chiavi condivise nei campi o creata tramite JOIN. Questo significa che le seguenti relazioni possono richiedere più query o applicazioni che eseguono il lavoro di assemblaggio delle relazioni da ulteriori query generali.

Nei database grafici, la relazione è un componente primario del modello di dati e l'attraversamento dal vertice al bordo al vertice e oltre è il meccanismo primario per l'esecuzione di query dei dati nel modello. Questo rende i database grafici maggiormente appropriati per eseguire le query che coinvolgono le relazioni come ad esempio "tutte le persone a cui piace la marca X e hanno un amico o un amico di un amico, che compra la marca Y". 

In JanusGraph, ogni grafico ha un nome univoco. Per creare, aprire, modificare ed eseguire la query di un grafico, utilizza il linguaggio Gremlin. Una query Gremlin è formata da comandi che possono manipolare o attraversare il grafico in vari modi per fornire il risultato che desideri.

## Vertici

Un vertice è un elemento grafico che rappresenta un oggetto. Una creazione del vertice ha un identificativo impostato dal motore del grafico e un'etichetta definita dall'utente. Un vertice può anche archiviare le proprietà che possono essere indicizzate e interrogate.

Per ulteriori informazioni su come creare, eseguire le query e su altri dettagli di implementazione dei vertici, consulta [Tinkerpop/Gremlin documentation](http://tinkerpop.apache.org/docs/3.2.3/reference/#_the_graph_structure).

## Bordi

Un bordo è un elemento grafico che rappresenta una relazione. Per creare un bordo, l'utente fornisce i vertici in entrata/uscita e un'etichetta. Al bordo sarà assegnato un identificativo dal motore del grafico. Un bordo può anche archiviare le proprietà che possono essere indicizzate e interrogate.

Per ulteriori informazioni su come creare, eseguire le query e su altri dettagli di implementazione dei bordi, consulta [Tinkerpop/Gremlin documentation](http://tinkerpop.apache.org/docs/3.2.3/reference/#_the_graph_structure).

## Proprietà

Una proprietà è una coppia chiave:valore che descrive, denomina o è associata a un bordo o un vertice. Sia i vertici che i bordi possono avere più proprietà. Ad esempio, se il vertice è del tipo 'human', possiamo assegnare le proprietà a tale vertice di 'name' e 'age'.

Le proprietà possono essere indicizzate in modo che i vertici e i bordi possano essere richiamati dalle rispettive proprietà, come l'ottenimento di tutti i vertici con lo stesso nome.

Le proprietà dei bordi possono essere tipo 'weight', in modo che un attraversamento grafico dei vertici tramite più bordi pesati differentemente possa avere un peso totale calcolato per il viaggio. 

## Attraversamento

L'attraversamento è un processo di analisi di una struttura del grafico. L'attraversamento è il processo che rileva e restituisce le informazioni sui bordi, i vertici e le loro proprietà. Per ulteriori informazioni sugli algoritmi e i passi di attraversamento differenti, consulta [Tinkerpop/Gremlin documentation on traversal](http://tinkerpop.apache.org/docs/3.2.3/reference/#traversal).

## Gremlin

Sono presenti due parti di Gremlin: il linguaggio e il server.

Gremlin, il linguaggio, è un linguaggio di attraversamento grafico che utilizzi per interagire ed eseguire query sul tuo grafico.

Gremlin, il server, è una specifica di un server che elabora le query del linguaggio Gremlin remote o locali. Un'implementazione di esso è parte di Apache Tinkerpop.

Per eseguire una query di linguaggio Gremlin, invia la tua query Gremlin al server Gremlin appropriato. In Compose, questo è un server in esecuzione come parte della tua distribuzione JanusGraph.

## Apache TinkerPop

Apache TinkerPop è un framework di calcolo grafico open source . TinkerPop è il sistema che modella i dati come un grafico e interagisce con i comandi del linguaggio Gremlin per creare, attraversare e manipolare i grafici. Per ulteriori informazioni su come unire tutti questi pezzi, consulta la documentazione TinkerPop/Gremlin in [graph system integration](http://tinkerpop.apache.org/docs/3.2.3/reference/#_graph_system_integration).
