---

copyright:
  years: 2016,2018
lastupdated: "2018-03-27"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Preisstruktur
{: #pricing}

## Basiskonfiguration
Ein {{site.data.keyword.composeForJanusGraph_full}}-Service beginnt mit einem Cluster von zwei JanusGraph Engine-Knoten mit jeweils 512 MB Hauptspeicher, was 2 Ressourceneinheiten entspricht. JaunsGraph Storage ist ein Cluster mit drei Knoten mit jeweils 5 GB Speicher, was 5 Ressourceneinheiten entspricht. Der Service _umfasst_ Replikation und Hochverfügbarkeit. In jeder JanusGraph Engine-Einheit und im Einzelpreis sind die Ressourcenkosten für beide Engineknoten _enthalten_. Entsprechend sind auch in der JanusGraph Storage-Einheit und im Einzelpreis die Kosten der Ressourcen für alle drei Speicherknoten enthalten.

Zur Basiskonfiguration gehören zudem zwei HAProxy-Portale für Zugriff, HTTPS und IP-Whitelisting. Sie haben jeweils 64 MB Hauptspeicher.

### Kosten
Die Basisservicekonfiguration hat einen Festpreis. Den Grundpreis in Ihrer Landeswährung finden Sie über die entsprechenden Katalogkacheln auf {{site.data.keyword.cloud_notm}}. Der Grundpreis in US-Dollar beträgt zum Beispiel $116/Monat. In diesem Preis sind die 5 Einheiten für das JanusGraph Storage-Cluster zu $90/Monat und 2 Einheiten für die JanusGraph Engine zu $26/Monat enthalten.


## Erweiterungsoptionen
Welche die optimale Konfiguration von Speicher und Hauptspeicher ist, variiert je nach Anwendungsfällen und Workloads. Wenn Sie zusätzlichen Speicher für große Datensätze oder zusätzlichen Hauptspeicher für komplexe Abfragen benötigen, können Sie die zugeordneten Ressourcen sowohl für den vom JanusGraph Storage-Cluster bereitgestellten Speicher als auch für den von den JanusGraph Engine-Knoten bereitgestellten Hauptspeicher erhöhen. 

In der Erhöhung der Speichercluster zu Einheiten von jeweils 1 GB und im Einzelpreis sind die Kosten für die Erweiterung der Ressourcen auf allen Knoten im Cluster _enthalten_. Das Skalieren des Speichers ist über die Registerkarte _Einstellungen_ des Service verfügbar.
 
In der Erhöhung der Engineknoten zu Einheiten von jeweils 256 MB Hauptspeicher und im Einzelpreis sind die Kosten für die Erweiterung der Ressourcen auf beiden Knoten _enthalten_. Derzeit ist eine Skalierung des Hauptspeichers nur durch Kontaktaufnahme mit dem Support verfügbar.

### Kosten
Jede zusätzliche JanusGraph Storage-Einheit (1 GB Speicher) und jede zusätzliche JanusGraph Engine-Einheit (256 MB Hauptspeicher) hat einen Einzelpreis, der auf der {{site.data.keyword.cloud_notm}}-Katalogkachel für den Service in Ihrer Landeswährung aufgelistet ist. In US-Dollar beträgt der Preis für jede JanusGraph Storage-Einheit $18 und für jede JanusGraph Engine-Einheit $13. Mit zunehmender _Gesamtgröße_ Ihrer {{site.data.keyword.composeForJanusGraph}}-Services verringert sich der Preis pro Einheit, wie aus der Tabelle zur [gestaffelten Preisstruktur](#tiered-pricing) hervorgeht.

## Gestaffelte Preisstruktur

### Gestaffelte Preisstruktur von {{site.data.keyword.composeForJanusGraph}} Storage

Anzahl der Einheiten für JanusGraph Storage|Einzelpreis
----------|-----------
5 - 9 Einheiten|Basiseinzelpreis -- $18,00 USD/Storage-Einheit
10 - 24 Einheiten|10% Ermäßigung -- $16,20 USD/Storage-Einheit
25 - 49 Einheiten|20% Ermäßigung -- $14,40 USD/Storage-Einheit
50 - 99 Einheiten|30% Ermäßigung -- $12,60 USD/Storage-Einheit
100 - 499 Einheiten|40% Ermäßigung -- $10,80 USD/Storage-Einheit
500 - 999 Einheiten|50% Ermäßigung -- $9,00 USD/Storage-Einheit
1.000 - 4.999 Einheiten|60% Ermäßigung -- $7,20 USD/Storage-Einheit
5.000+ Einheiten|70% Ermäßigung -- $5,40 USD/Storage-Einheit
{: caption="Tabelle 1. Gestaffelte Preisstruktur von {{site.data.keyword.composeForJanusGraph}} Storage" caption-side="top"}

### Gestaffelte Preisstruktur von {{site.data.keyword.composeForJanusGraph}} Engine

Anzahl der Einheiten für JanusGraph Engine|Einzelpreis
----------|-----------
2 - 9 Einheiten|Basiseinzelpreis -- $13,00 USD/Engine-Einheit
10 - 24 Einheiten|10% Ermäßigung -- $11,70 USD/Engine-Einheit
25 - 49 Einheiten|20% Ermäßigung -- $10,40 USD/Engine-Einheit
50 - 99 Einheiten|30% Ermäßigung -- $9,10 USD/Engine-Einheit
100 - 499 Einheiten|40% Ermäßigung -- $7,80 USD/Engine-Einheit
500 - 999 Einheiten|50% Ermäßigung -- $6,50 USD/Engine-Einheit
1.000 - 4.999 Einheiten|60% Ermäßigung -- $5,20 USD/Engine-Einheit
5.000+ Einheiten|70% Ermäßigung -- $3,90 USD/Engine-Einheit
{: caption="Tabelle 2. Gestaffelte Preisstruktur von {{site.data.keyword.composeForJanusGraph}} Engine" caption-side="top"}
