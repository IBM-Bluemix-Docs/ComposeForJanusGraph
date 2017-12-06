---

Copyright:
  Years: 2017
lastupdated: "2017-09-05"
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

Der Datenbanktyp, der vom Service angeboten wird, und die Datenbankversion, die Ihr Service verwendet.

### Name

Eine interne ID für den Service.

### Nutzung

Die Größe Ihrer Datenbank und der von Ihrem Serviceplan bereitgestellte Speicherplatz.


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

