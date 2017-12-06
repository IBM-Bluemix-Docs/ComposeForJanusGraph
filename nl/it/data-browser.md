---

copyright:
  years: 2017
lastupdated: "2017-09-01"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Utilizzo del browser dati JanusGraph 

Esplorare i tuoi dati del grafico dalla riga di comando può essere un'attività complessa e può essere difficile creare passaggi. Può essere difficile visualizzare i risultati, restituiti come output di testo o JSON, in termini di relazioni del grafico utilizzabili. Qui è dove torna utile il browser per JanusGraph in Compose.

Il browser dati per {{site.data.keyword.composeForJanusGraph_full}} combina un modo facile di utilizzare il builder di query con le schede della risposta della query avanzate che formano il builder. Ogni scheda registra la query e visualizza i risultati di una vista JSON interattiva e un grafico visualizzato che può essere esplorato, correlato alla vista JSON. Ogni scheda può aiutarti a rifinire la tua prossima query.

## Introduzione al browser dati.

Il link al browser dati è ubicato nella pagina _Dashboard Overview_ del tuo servizio. Fai clic sul link per caricare l'interfaccia in una nuova scheda del browser.

Questa è una vista del browser dati dopo che è stata eseguita una prima query.

![Una vista del browser dati quando avviato](./images/databrowser_taggedFullscreenbrowser.png "Una vista del browser dati quando avviato che mostra il builder di query, l'output della query nei formati grafici e JSON e un messaggio di benvenuto in un'esercitazione.")

Il browser dati mostra il builder di query **(1)**, dove crei, modifichi ed esegui le tue query. Sotto il builder di query è presente una scheda di risposta della query **(2)**. Le nuove schede sono inserite all'inizio della pila di schede. La scheda iniziale precedente era un'introduzione interattiva al browser **(3)**, che viene visualizzata quando avvii il browser.

## Il builder di query

Il builder di query è un editor a più righe con l'evidenziazione della sintassi che ti aiuta a creare gli script Gremlin.

![Builder di query con una query di esempio](./images/databrowser_taggedquerybuilder.png "Il builder di query con una query di esempio")

## Schede di risposta e pila di schede di risposta

Ogni query genera una scheda di risposta che contiene la tua query, una risposta JSON e una visualizzazione grafica dei risultati della query se disponibili. L'inizio di ogni scheda visualizza la query che è stata eseguita.

![Intestazione della scheda di risposta di esempio.](./images/databrowser_querybar.png)

La scheda visualizza la query che stata eseguita **(1)**, il pulsante **Copy** **(2)**, il pulsante **Collapse**/**Expand** **(3)** e il pulsante **Close** **(4)**.

Come esegui altre query, ognuna di esse genera una nuova scheda di risposta, con visualizzata per prima la scheda di risposta più recente. Se la pagina ci impiega molto tempo o se noti che le prestazioni del browser dati stanno diminuendo, puoi utilizzare il pulsante **Collapse** per salvare alcuni frame. Se non hai più bisogno dei risultati in una scheda, puoi chiuderla completamente. La chiusura di una scheda di risposta non elimina tutti i dati del grafico.

## risposta della query: Il visualizzatore JSON

Il visualizzatore JSON è una vista di testo con sintassi evidenziata della risposta. Le righe sono numerate per aiutarti a navigare tra i risultati. Dove viene nidificato il documento JSON, vengono visualizzate delle piccole frecce. Puoi fare clic sulle frecce per piegare le sezioni nidificate:

![Contenuto JSON piegato](./images/databrowser_queryresponse.png)

La vista JSON include anche i filtri che possono venire applicati per gestire quali informazioni visualizzare. Per selezionare i filtri, fai clic sui pulsanti **Label**, **Type** e **Properties**. Puoi selezionare più filtri.

![Sfoglia i filtri nell'operazione](./images/databrowser_filteractions.png)

## risposta della query: Il visualizzatore 

Se il tuo risultato di query può essere visualizzato, la scheda visualizza un grafico che mostra i vertici e i bordi dalla risposta della query. Fai clic su un vertice per visualizzarne le proprietà. Puoi fare clic sui vertici e trascinarli per spostarli e bloccarli in una posizione.

Ad esempio, utilizzando il database di esempio Graph of the Gods, la seguente è una query per trovare i vertici con l'etichetta 'God':

```groovy
def g=ConfiguredGraphFactory.open("example").traversal();
g.V().has(T.label, "god");
```

La query produce la seguente visualizzazione e scheda di risposta, che mostra tutti i vertici nel grafico che rappresentano gods:

![Visualizzazione grafica dei vertici "god".](./images/databrowser_visualization.png)

La seguente query produce un risultato che mostra i vertici 'god' insieme ai bordi che provengono da essi e i vertici in cui vanno questi bordi:

```groovy
def g=ConfiguredGraphFactory.open("example").traversal();
g.V().has(T.label, "god").outE().inV().path();
```

La visualizzazione grafica dei risultati della query sarà simile a quanto segue:

![Visualizzazione grafica dei vertici gods, i rispettivi bordi 'out' e i vertici 'in'.](./images/databrowser_edgesvertices.png)

### Il comando .path() 

Il visualizzatore esegue il rendering dei risultati JSON mostrati nel visualizzatore JSON, in modo che vengano visualizzati solo i vertici e i bordi restituiti. Se la rotta della query attraversa solo i vertici, vengono restituiti solo i vertici, ma se include i bordi, essi vengono inclusi nei risultati. Esistono diversi modi per popolare i risultati con i bordi. Un metodo efficace è di utilizzare la funzione `path()`. Quando aggiunta a una query Gremlin, `path()` restituisce la rotta utilizzata per ottenere i vertici nella risposta della query.

La documentazione Gremlin in [path-step](http://tinkerpop.apache.org/docs/current/reference/#path-step) ha ulteriori informazioni sulla funzione `path()`.
{: .tip}

Ad esempio, la seguente query restituisce solo vertici:

```groovy
def g=ConfiguredGraphFactory.open("example").traversal();
g.V().outE().inV()
```

Anche la visualizzazione risultante contiene solo i vertici.

![Risultato di esempio di una visualizzazione grafica senza bordi.](./images/databrowser_visualization2.png)

Puoi modificare la risposta della query aggiungendo `path()` alla stessa query.

```groovy
def g=ConfiguredGraphFactory.open("example").traversal();
g.V().outE().inV().path()
```

La query produce ora una risposta che contiene sia vertici che bordi.

![Risultato di esempio di un risultato di visualizzazione grafica utilizzando `.path()`.](./images/databrowser_visualization3.png)

## Gestione dei risultati 'null' 

Alcuni comandi nel browser possono restituire un risultato `null`. Questo può succedere quando il valore restituito non è al momento serializzabile. L'esempio più comune è un comando o un'espressione che restituisce un grafico, inclusi i metodi `open` e `create` della classe `ConfiguredGraphFactory`. Sebbene venga visualizzata una risposta `null`, i valori effettivi sono intatti in JanusGraph e sono disponibili per l'utilizzo in una query. Quando utilizzi `ConfiguredGraphFactory`, estendi il tuo comando per restituire i vertici e i bordi per garantire che venga restituita una risposta JSON.
