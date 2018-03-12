---

Copyright:
  years: 2017,2018
lastupdated: "2017-12-12"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Einführung in Compose for JanusGraph
{: #getting-started-with-compose-for-janusgraph}

{{site.data.keyword.composeForJanusGraph_full}} ist eine skalierbare Diagrammdatenbank, die für die Speicherung und Abfrage stark vernetzter Daten optimiert ist, die als Millionen oder Milliarden von Vertices und Kanten modelliert werden. Der einfache und effiziente Abruf von Daten aus diesen komplexen Strukturen wird mit der Apache Tinkerpop(TM)-Kompatibilität von JanusGraph ermöglicht, die die Ausführung effizienter Abfragen zulässt, die mit einer traditionellen relationalen Datenbank schwer oder unmöglich wären. {{site.data.keyword.composeForJanusGraph}} verwaltet JanusGraph für Sie und macht es dadurch noch besser. Die Datenbank bietet ein einfaches, skalierbares Bereitstellungssystem, das Hochverfügbarkeit und Redundanz, automatisierte und dauerhaft aktive Sicherungen und vieles mehr bietet.
{:shortdesc}

Eine Übersicht über {{site.data.keyword.composeForJanusGraph}} finden Sie in [JanusGraph - Konzepte](./janusgraph-concepts.html)

## Compose for JanusGraph-Serviceinstanz erstellen

[Erstellen Sie eine {{site.data.keyword.composeForJanusGraph}}-Instanz](https://console.bluemix.net/catalog/services/compose-for-janusgraph/).

Wenn Sie eine Instanz des Service erstellen, achten Sie darauf, sowohl einen Namen für den Service als auch einen Namen für den Berechtigungsnachweis auszuwählen. Lassen Sie den Service ungebunden; Sie können später eine Anwendung mit Ihrem Service verbinden, indem Sie die Berechtigungsnachweise verwenden, die bei der Bereitstellung des Service angegeben wurden. Die verschiedenen Werte für die Berechtigungsnachweise und deren Verwendung in einer Anwendung werden detailliert im Abschnitt [{{site.data.keyword.cloud}}-Anwendung verbinden](./connecting-bluemix-app.html) beschrieben.

Bei der Bereitstellung Ihrer Serviceinstanz können Sie den Plan *Standard* oder *Enterprise* auswählen. Mit dem Plan *Enterprise* können Sie Ihre Instanz in einem {{site.data.keyword.composeEnterprise}}-Cluster bereitstellen. {{site.data.keyword.composeEnterprise}} stellt die für die Konformität mit Enterprise erforderliche Sicherheit und Isolation bereit und stellt mithilfe eines dedizierten Netzbetriebs die Leistung der bereitgestellten Datenbanken sicher. Weitere Details finden Sie in der [Dokumentation zu Compose Enterprise](../ComposeEnterprise/index.html).

## Compose for JanusGraph verwalten

Sie können Ihren Service über das Service-Dashboard verwalten. Hier finden Sie Informationen zu Ihrer {{site.data.keyword.cloud_notm}} Compose-Datenbank und dazu, wie Sie eine Verbindung zu ihr herstellen. Außerdem können Sie:
- Ihre Sicherungen verwalten
- Ihrem Service mehr Ressourcen zuordnen
- Das Servicekennwort ändern
- Whitelists verwenden, um den Zugriff auf Ihre Datenbank zu beschränken. 
Weitere Informationen finden Sie im Abschnitt [Einstellungen](./dashboard-settings.html).

## Verbindung zu Compose for JanusGraph herstellen

Sie können mit den Berechtigungsnachweisen, die zusammen mit Ihrem Service erstellt werden, oder mithilfe der Verbindungszeichenfolgen und der Befehlszeile, die auf der Registerkarte *Übersicht* Ihres Service-Dashboards bereitgestellt werden, eine Verbindung zu dem Service herstellen.

Wenn Sie außerhalb von {{site.data.keyword.cloud_notm}} eine Verbindung zu Ihrem Service herstellen wollen, können Sie dazu die bereitgestellten Verbindungszeichenfolgen oder die Befehlszeile verwenden. Weitere Informationen dazu finden Sie im Abschnitt [Verbindung zu JanusGraph herstellen](./connecting-external.html).

## {{site.data.keyword.cloud_notm}}-Anwendung verbinden

Um eine {{site.data.keyword.cloud_notm}}-Anwendung mit Ihrem Service zu verbinden, verwenden Sie die Berechtigungsnachweise, die zusammen mit dem Service erstellt wurden. Informationen zum Herstellen einer Verbindung von einer {{site.data.keyword.cloud_notm}}-Anwendung zu einem {{site.data.keyword.composeForJanusGraph}}-Service finden Sie im Abschnitt [{{site.data.keyword.cloud_notm}}-Anwendung verbinden](./connecting-bluemix-app.html).

## JanusGraph-Datenbrowser verwenden

Die Untersuchung Ihrer Diagrammdaten über die Befehlszeile kann eine komplexe Aufgabe darstellen. Der Datenbrowser für {{site.data.keyword.composeForJanusGraph}} verbindet einen benutzerfreundlichen Abfragebuilder mit umfangreichen Abfrageantwortkarten, die die Ergebnisse Ihrer Abfragen sowohl als interaktive JSON-Ansicht als auch als visualisiertes Diagramm anzeigen. Weitere Informationen finden Sie im Abschnitt [JanusGraph-Datenbrowser verwenden](./data-browser.html).
