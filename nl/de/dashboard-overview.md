---

Copyright:
  years: 2017,2018
lastupdated: "2017-11-15"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Übersicht über den Service

Auf der Seite _Übersicht_ finden Sie Informationen zu Ihrer {{site.data.keyword.cloud}} Compose-Datenbank. Die Übersicht enthält die zentralen Identifikationsinformationen sowie die aktuelle Ressourcennutzung. Außerdem finden Sie einen Abschnitt für Verbindungszeichenfolgen, die Sie mit Tools verwenden können, und Angaben zum Herstellen von Verbindungen zu Ihrer Datenbank mithilfe von Tools.

## Bereitstellungsdetails

Die Anzeige _Bereitstellungsdetails_ enthält Details zu Ihrem Service.

![Bereitstellungsdetails](./images/janusgraph-deployment-details.png "Ansicht der Anzeige 'Bereitstellungsdetails'")

### Typ

Der Datenbanktyp, der vom Service angeboten wird, und die Datenbankversion, die Ihr Service verwendet. Wenn eine neuere Datenbankversion verfügbar ist, wird eine Benachrichtigung zusammen mit einem Link zum Abschnitt [Upgradeversion](/docs/services/ComposeForJanusGraph/dashboard-settings.html#upgrade-version) Ihres Service-Dashboards angezeigt.

### Name

Eine interne ID für den Service.

### Nutzung

Die Größe Ihrer Datenbank und der von Ihrem Serviceplan bereitgestellte Speicherplatz.

## Aktuelle Jobs

Durch administrative Änderungen an Ihrem Service (wie Skalieren oder Erstellen einer manuellen Sicherung) starten Sie einen Job. Während der Ausführung eines Jobs, wird auf der Seite _Übersicht_ das Fenster _Aktuelle Jobs_ mit dem Jobnamen und einer Fortschrittsleiste angezeigt. Wenn der Job abgeschlossen ist, wird das Fenster _Aktuelle Jobs_ auf der Seite _Übersicht_ nicht mehr angezeigt.

## Verbindungszeichenfolgen

Verbindungszeichenfolgen können von bestimmten Clientbibliotheken verwendet werden und enthalten alle Informationen, die andere Bibliotheken zum Herstellen einer Verbindung benötigen. Informationen dazu, wie sich mithilfe einer Verbindungszeichenfolge eine Verbindung zu Ihrem Service herstellen lässt, finden Sie im Abschnitt [Externe Anwendung verbinden](./connecting-external.html).

Die einzelnen Verbindungszeichenfolgen für Ihren Service befinden sich jeweils auf einer eigenen Registerkarte der Anzeige _Verbindungszeichenfolgen_.

### Sitzung

Sie können mithilfe eines Sitzungs-URI ein Berechtigungstoken abrufen, das 60 Minuten gültig ist. Sie sollten die bereitgestellten Token verwenden, wenn Sie über die HTTPS- oder WebSocket-URIs Aufrufe an die Bereitstellung vornehmen.

### HTTPS

Dies ist die grundlegende Verbindungszeichenfolge für eine JanusGraph-Bereitstellung. Um eine HTTPS-Verbindungszeichenfolge verwenden zu können, müssen Sie die Berechtigungsnachweise des Benutzers mit Verwaltungsaufgaben bereitstellen, damit eine Verbindung zum Server hergestellt werden kann. Weitere Informationen zur Verwendung von HTTPS finden Sie im Abschnitt [Diagramme mit HTTPs erstellen und traversieren](./tutorial-https.html).

### WebSocket

Mit den WebSocket-URIs können Sie eine lange laufende Sitzung in der JanusGraph-Bereitstellung erstellen. Diese haben das Präfix `wss:`, das angibt, dass die Verbindungen mit HTTPS gesichert werden. Für diese Verbindungszeichenfolgen ist eine Basisauthentifizierung mithilfe der Berechtigungsnachweise des Benutzers mit Verwaltungsaufgaben erforderlich, damit eine Verbindung zum Server hergestellt werden kann.

Einige Bibliotheken für JanusGraph verwenden WebSockets als ihre Verbindung zum Server. Für die Arbeit mit {{site.data.keyword.composeForJanusGraph}} müssen sie in der Lage sein, die Basisauthentifizierung und WSS (sichere WebSockets über TLS) zu verwenden.

### YAML-Datei für Gremlin-Konsole

Sie können eine der bereitgestellten Konfigurationen verwenden, um mithilfe der Gremlin-Konsole eine Verbindung zu {{site.data.keyword.composeForJanusGraph}} herzustellen. Weitere Informationen zur Verwendung der YAML-Datei für die Gremlin-Konsole finden Sie im Abschnitt [Diagramme mit der Gremlin-Konsole erstellen und traversieren](./tutorial-gremlin-console.html).


## Instanzverwaltungs-API

Sie können Ihren {{site.data.keyword.composeForJanusGraph}}-Service über die {{site.data.keyword.cloud_notm}} Compose-API verwalten.

### Basisendpunkt

Der Basisendpunkt setzt sich aus der Region, in der sich der Service befindet, und der Serviceinstanz-ID zusammen. Er steht am Anfang von jedem Endpunkt.

### Bereitstellungs-ID

Die Bereitstellungs-ID wird für die meisten Aufrufe benötigt und gibt eine bestimmte Bereitstellungsinstanz an.

### Referenz

Zusätzliche Dokumentation und Referenz zur Verwendung der {{site.data.keyword.cloud_notm}} Compose-API für alle {{site.data.keyword.cloud_notm}} Compose-Services finden Sie in [Die {{site.data.keyword.cloud_notm}} Compose-API](https://www.compose.com/articles/the-ibm-cloud-compose-api/).
