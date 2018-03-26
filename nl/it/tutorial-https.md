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

# Creazione e trasmissione di un grafico utilizzando HTTPS

{{site.data.keyword.composeForJanusGraph_full}} viene fornito con un database grafico di esempio, 'The Graph of the Gods'. Questa esercitazione utilizza questo database di esempio per illustrare alcuni concetti di JanusGraph. Segui questa procedura per creare, aprire e attraversare il grafico, utilizzando CURL per inviare i comandi Gremlin. Per visualizzare una rappresentazione grafica, consulta la documentazione [Getting Started](http://docs.janusgraph.org/latest/getting-started.html) JanusGraph. 

## 1. Collegati a Compose for JanusGraph

Per collegare il tuo servizio {{site.data.keyword.composeForJanusGraph}} utilizzando CURL devi utilizzare una stringa di connessione. La stringa di connessione include il nome utente, la password, il server e il numero di porta necessario per il collegamento al tuo servizio. Puoi trovarli nella pagina _Overview_ del tuo dashboard del servizio.

Prima di iniziare l'esercitazione, verifica di poterti collegare utilizzando la stringa d connessione trasmettendo un comando Gremlin semplice:

```
curl -XPOST -d '{"gremlin" : "1+1" }' "<CONNECTION STRING>"
```

Se la richiesta ha esito positivo, JanusGraph restituisce una risposta simile a quanto segue:

```
{
  requestId":"d2fe6ac7-95d9-4c79-a254-123648cad54d",
  "status":{
    "message": "",
    "code": 200,
    "attributes": {}
  },
  "result": {
    "data":[2],
    "meta":{}
  }
}
```

Sei ora pronto ad iniziare ad utilizzare The Graph of the Gods.

Aggiungi la tua stringa di connessione alla fine di ogni comando CURL nell'esercitazione.
{: .tip}

## 2. Crea il grafico

{{site.data.keyword.composeForJanusGraph}} ha un factory grafico dedicato per la creazione, l'apertura e la chiusura dei grafici. L'utilizzo di questo factory - `ConfiguredGraphFactory` - significa che non devi conoscere il meccanismo di archiviazione sottostante, per cui la creazione di un nuovo grafico è solo una questione di denominazione. Inizia creando un nuovo grafico, denominato _Example_.

Il sandbox JanusGraph richiede che dichiari tutte le variabili che stai utilizzando. Per dichiarare le variabili, utilizza la parola chiave `def`. Ad esempio, per dichiarare la variabile graph:

```
curl -XPOST -d '{"gremlin": "def graph=ConfiguredGraphFactory.create(\"example\");0;"}'
```

I nomi del grafico {{site.data.keyword.composeForJanusGraph}} possono includere solo caratteri alfanumerici e di sottolineatura.
{: .tip}

Tieni presente che il comando curl termina con `;0;`. Questa è una soluzione temporanea, perché l'API della richiesta HTTP emette un errore (`{"message":"Cannot get namespace of root","Exception-Class":"java.lang.IllegalArgumentException"}`) anche se l'operazione è stata completata correttamente. Questo influenza solo il tipo di grafico quando il codice tenta di restituirlo.

## 3. Apri il grafico

Dopo aver creato un grafico, per utilizzarlo, devi aprirlo. Puoi farlo richiamando `open()` in `JanusGraphConfiguredFactory`. Apriamo il nostro grafico 'example'.

```
curl -XPOST -d '{"gremlin": "def graph=ConfiguredGraphFactory.open(\"example\");0;"}'
```

## 4. Carica The Graph of the Gods

Per caricare il grafico di esempio, richiama il metodo `GraphOfTheGods.loadWithoutMixedIndex()`, trasmettendolo al tuo grafico 'example' e il modello viene creato per tuo conto.

```
curl -XPOST -d '{"gremlin": "def graph=ConfiguredGraphFactory.open(\"example\"); GraphOfTheGodsFactory.loadWithoutMixedIndex(graph,true);"}'
```

## 5. Prepara l'attraversamento del grafico

Ora che hai creato e aperto un grafico e caricato alcuni dati in esso, puoi esplorarlo. Per muoverti nel grafico o eseguirne una query, hai bisogno di un'origine di attraversamento, che si ottiene richiamando `traversal()` nell'oggetto del grafico. Questo significa che il prefisso completo di tutti i comandi deve essere:

```
curl -XPOST -d '{"gremlin": "def graph=ConfiguredGraphFactory.open(\"example\");def g=graph.traversal();0;"}'
```

L'utilizzo di `graph` per il grafico e di `g` per l'origine di attraversamento è comune. Se hai bisogno soltanto dell'origine di attraversamento, puoi comprimere quanto segue in:

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal();0;"}'
```

## 6. Attraversa il grafico e trova Saturn

Il grafico The Graph of the Gods contiene un indice globale delle proprietà del nome. Questo tipo di indice è spesso il tuo primo passo per arrivare a un punto particolare nel grafico. Ad esempio, per trovare "saturn" nel grafico:

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next()"}'
```

Questo comando restituisce il vertice che ha la proprietà `name: saturn`.

```
{
  "requestId":"34d349a7-fa8e-4268-8b8d-a8adc0056bf3",
  "status":{
    "message": "",
    "code": 200,
    "attributes": {}
  },
  "result": {
    "data": [
      {
        "id":4184,
        "label":"titan",
        "type":"vertex",
        "properties":{
          "name":[
            {
              "id":"16z-388-sl",
              "value":"saturn"
            }
          ],
          "age":[
            {
              "id":"1l7-388-35x",
              "value":10000
            }
          ]
        }
      }
    ],
    "meta": {}
  }
}
```

## 7. Attraversa il grafico per trovare i figli di Saturn

Per promuovere ulteriormente l'esempio, utilizzando il vertice "saturn" definendolo in una variabile 'saturn', puoi chiedere quali vertici archivino un bordo con l'etichetta 'father' che puntino ad essa. Utilizzando il metodo `in("father")` selezioni quei bordi in entrata e utilizzando `values("name")` restituisci il valore della proprietà del nome di tale vertice.

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); ;g.V(saturn).in(\"father\").values(\"name\")"}'
```

La query restituisce i nomi dei figli di Saturn nel grafico, anche se c'è solo un figlio: "jupiter".

```
{
  "requestId":"2ad6c01e-f724-4e0a-8895-4e8e368fb7ca",
  "status":{
    "message": "",
    "code": 200,
    "attributes": {}
  },
  "result": {
    "data":[
      "jupiter"
    ],
    "meta": {}
  }
}
```

Se includi l'operazione `in("father")` una seconda volta, il tuo attraversamento ti porta al nipote del vertice Saturn.

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); ;g.V(saturn).in(\"father\").in(\"father\").values(\"name\")"}'
```

Utilizzando questa query, trovi che il nome del nipote di Saturn è 'hercules'.

## 8. Attraversa il grafico - Ubicazione

In GraphOfTheGods, alcuni bordi hanno una proprietà "place", composta da latitudine e longitudine. Questa proprietà può essere utilizzata per geolocalizzare gli eventi. I bordi etichettati "battled" utilizzano la proprietà di posizionamento per rappresentare dove sono avvenute le battaglie. Per trovare tutte le battaglie che hanno avuto luogo nel raggio di 50 chilometri da Atene (latitudine: 37.97 e longitudine: 23.72), ricerca i bordi del grafico per tutti i valori della proprietà "place" che rientrano nel raggio di queste coordinate.

Puoi selezionare questi bordi utilizzando `has("place", geoWithin(Geoshape.circle(37.97, 23.72, 50)))`.

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); g.E().has(\"place\", geoWithin(Geoshape.circle(37.97, 23.72, 50)))"}'
```

La query restituisce l'etichetta del bordo per i vertici in entrata e in uscita e i rispettivi identificativi. Per rendere la risposta immediatamente leggibile e per trovare chi ha partecipato a queste due battaglie, possiamo utilizzare `as()` per nascondere i valori restituiti come _god1_ e _god2_ e quindi `select()` per richiamare questi valori, utilizzando `by("name")` per estrarre i nomi delle due divinità coninvolte in ogni battaglia.

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); g.E().has(\"place\", geoWithin(Geoshape.circle(37.97, 23.72, 50))).as(\"source\").inV().as(\"god2\").select(\"source\").outV().as(\"god1\").select(\"god1\", \"god2\").by(\"name\")"}'
```

```
"result":{
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
```

## 9. Attraversa il grafico - Vertici

In un passo precedente hai utilizzato due elementi `in()` per identificare che Hercules era il nipote di Saturn. L'attraversamento può anche essere espresso come un loop, perché Gremlin può ripetere `repeat` un predicato:

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); def hercules=g.V(saturn).repeat(__.in(\"father\")).times(2).next()"}'
```

Puoi eseguire operazioni avanzate che utilizzano il vertice Hercules come punto di partenza. Ad esempio, puoi:

- Attraversare i bordi etichettati "father" e "mother" e determinare il "type" di genitori di Hercules:

  ```
  curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); def hercules=g.V(saturn).repeat(__.in(\"father\")).times(2).next(); g.V(hercules).out(\"father\", \"mother\").label();" }'
  ```

- Attraversare i bordi "battled" per visualizzare con chi ha combattuto Hercules:

  ```
  curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); def hercules=g.V(saturn).repeat(__.in(\"father\")).times(2).next(); g.V(hercules).out(\"battled\").valueMap()" }' 
  ```

- Attraversare i bordi "battled" per visualizzare con chi ha combattuto Hercules più di una volta:

  ```
  curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); def hercules=g.V(saturn).repeat(__.in(\"father\")).times(2).next(); g.V(hercules).outE(\"battled\").has(\"time\", gt(1)).inV().values(\"name\")" }' 
  ```
  
