---

copyright:
  years: 2016,2018
lastupdated: "2018-01-03"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Prezzi
{: #pricing}

## Configurazione di base
Un servizio {{site.data.keyword.composeForJanusGraph_full}} viene fornito come un cluster con due nodi del motore JanusGraph, ognuno con 512MB di memoria, che equivale a 2 unità di risorse. L'archiviazione JaunsGraph è un cluster di tre nodi in cui ogni nodo ha 5GB di archiviazione, che equivale a 5 unità di risorse. Il servizio _include_ la replica e l'elevata disponibilità, per cui ogni unità  e il prezzo per unità _include_ il costo delle risorse in entrambi i nodi del motore. Allo stesso modo, ogni prezzo per unità e unità di archiviazione JanusGraph include il costo delle risorse nei tre nodi del motore.

La configurazione di base include anche i due portali HAProxy per l'accesso, HTTPS e la whitelist IP. Hanno 64MB di memoria ognuno.

### Costo
La configurazione del servizio di base a un prezzo fissato. Consulta i tile del catalogo {{site.data.keyword.cloud_notm}} per i prezzi di base nella tua valuta locale. Ad esempio, il prezzo di base in dollari US è $116/mese. Questo include le 5 unità del cluster di archiviazione JanusGraph a $90/mese le 2 unità del motore JanusGraph a $26/mese.


## Opzioni di espansione
Il trovare la configurazione ottimale di memoria e archiviazione varierà tra i casi di utilizzo e i carichi di lavoro. Se hai bisogno di ulteriore archiviazione o memoria per il tuo servizio, puoi aumentare le risorse assegnate all'archiviazione fornita dal cluster di archiviazione JanusGraph e la memoria fornita dai nodi del motore JanusGraph. 

I cluster di archiviazione vengono incrementati in unità di 1GB e il prezzo per unità _include_ il costo dell'incremento delle risorse in tutti i nodi nel cluster. Il ridimensionamento dell'archiviazione è disponibile nella scheda _Settings_ del servizio.
 
I nodi del motore vengono incrementati per unità di 256MB di memoria e il prezzo per unità _include_ il costo dell'incremento su entrambi i nodi. Al momento, il ridimensionamento è disponibile solo contattando il supporto.

### Costo
Ogni unità aggiuntiva (1GB) di archiviazione JanusGraph e ogni unità di memoria del motore JanusGraph (256MB) ha un prezzo per unità che viene elencato nella tua valuta corrente nel tile del catalogo {{site.data.keyword.cloud_notm}} per il servizio. In dollari US, ogni unità di archiviazione JanusGraph è $18 e ogni unità del motore JanusGraph è $13. Quando la dimensione _totale_ di tutti i tuoi servizi {{site.data.keyword.composeForJanusGraph}} aumenta, il prezzo per unità diminuisce, come mostrato nei [prezzi a livelli](#tiered-pricing).

## Prezzi a livelli

### Prezzi a livelli dell'archiviazione {{site.data.keyword.composeForJanusGraph}} 

Numero di unità di archiviazione JanusGraph|Prezzo per unità
----------|-----------
5 - 9 unità|prezzo per unità di base -- $18.00 USD/Unità di archiviazione
10 - 24 unità|10% di riduzione -- $16.20 USD/Unità di archiviazione
25 - 49 unità|20% di riduzione -- $14.40 USD/Unità di archiviazione
50 - 99 unità|30% di riduzione -- $12.60 USD/Unità di archiviazione
100 - 499 unità|40% di riduzione -- $10.80 USD/Unità di archiviazione
500 - 999 unità|50% di riduzione -- $9.00 USD/Unità di archiviazione
1.000 - 4.999 unità|60% di riduzione -- $7.20 USD/Unità di archiviazione
5.000+ unità|70% di riduzione -- $5.40 USD/Unità di archiviazione
{: caption="Tabella 1. Prezzi a livelli di {{site.data.keyword.composeForJanusGraph}} " caption-side="top"}

### Prezzi a livelli della memoria {{site.data.keyword.composeForJanusGraph}} 

Numero di unità di memoria JanusGraph |Prezzo per unità
----------|-----------
2 - 9 unità|prezzo per unità di base -- $13.00 USD/Unità di motore 
10 - 24 unità|10% di riduzione -- $11.70 USD/Unità di motore 
25 - 49 unità|20% di riduzione -- $10.40 USD/Unità di motore 
50 - 99 unità|30% di riduzione -- $9.10 USD/Unità di motore 
100 - 499 unità|40% di riduzione -- $7.80 USD/Unità di motore 
500 - 999 unità|50% di riduzione -- $6.50 USD/Unità di motore 
1.000 - 4.999 unità|60% di riduzione -- $5.20 USD/Unità di motore 
5.000+ unità|70% di riduzione -- $3.90 USD/Unità di motore 
{: caption="Tabella 2. Prezzi a livelli della memoria {{site.data.keyword.composeForJanusGraph}} " caption-side="top"}
