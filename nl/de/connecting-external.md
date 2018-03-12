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

# Verbindung zu JanusGraph herstellen

JanusGraph unterstützt Verbindungen mithilfe von HTTP-Anforderungen oder über WebSockets. Alle Verbindungen werden über HTTPS ausgeführt und erfordern eine Authentifizierung. HTTP-basierte Anforderungen unterstützen Basis- und Tokenauthentifizierung. Für alles andere als einen einmaligen Aufruf für die JanusGraph-Bereitstellung führt die Verwendung von WebSockets oder Tokenauthentifizierung zu einer besseren Leistung. Die Basisauthentifizierung ist ein kostenintensiver Prozess, der Anforderungen verlangsamt.

Weitere Informationen zur Verwendung von HTTPS sowie zum Erstellen und Öffnen von Diagrammen finden Sie im Abschnitt [Diagramme mit HTTPS erstellen und traversieren](./tutorial-https.html).
{: .tip}

Die eigentliche Kommunikation zwischen dem Client und der Datenbank erfolgt dadurch, dass der Client Anforderungen in Gremlin sendet, einem Dialekt der Sprache Groovy, der für Abfragen von Diagrammstrukturen angepasst wurde, und dass die Datenbank auf diese Abfragen antwortet. Sie können Anforderungen auch über die Gremlin-Konsole senden.

Weitere Informationen zum Abrufen, Installieren, Konfigurieren und Verwenden der Gremlin-Konsole finden Sie im Abschnitt [Diagramme mit der Gremlin-Konsole erstellen und traversieren](./tutorial-gremlin-console.html).
{: .tip}

## HTTP-Anforderungen

Wenn Sie HTTP-Anforderungen für die Verbindung zum Server verwenden, kann ein einzelner Endpunkt für die Kommunikation mit JanusGraph verwendet werden, der auf allen verfügbaren Knoten in der Bereitstellung im Umlaufverfahren eingerichtet wird. Er umfasst einen Hostnamen und einen Port.

Sie können Ihre HTTPS-Verbindungszeichenfolgen von der Übersichtsseite Ihres Service-Dashboards abrufen. Für die Verbindungszeichenfolge ist eine Basisauthentifizierung mithilfe der Berechtigungsnachweise des Benutzers mit Verwaltungsaufgaben erforderlich, damit eine Verbindung zum Server hergestellt werden kann.

Um über eine HTTP-Anforderung eine Verbindung zu {{site.data.keyword.composeForJanusGraph}} herzustellen, müssen Sie die HTTP-Methode POST mit dem Endpunkt verwenden und ein in JSON formatiertes Dokument mit einem einzelnen Eintrag mit dem Schlüssel "gremlin" und einem Wert der zu sendenden Gremlin-Abfrage senden. 

```json
{
    "gremlin": "a gremlin query would go here"
}
```

Gremlin ist eine umfangreiche Traversierungssprache, doch ein in ihr enthaltenes Programm kann so einfach wie "1+1" sein. Um dies als Script in einem `curl`-Befehl zu übergeben, muss der Datenteil einer POST-Anforderung wie folgt lauten:

```
'{"gremlin" : "1+1" }'
``` 

Wenn Sie dasselbe Script mehrmals an den Server senden, können Sie die Leistung verbessern, indem Sie ein optionales Wörterverzeichnis namens "bindings" mit einer Übersicht der Variablen bereitstellen, die für das Gremlin-Script verfügbar sind. Dies steigert die Leistung, weil der Server das Gremlin-Script nicht erneut kompilieren muss.
{: .tip}

Weitere Informationen zum HTTP-Protokoll finden Sie in der Dokumentation zu Apache Tinkerpop im [Abschnitt zum Verbinden über REST](http://tinkerpop.apache.org/docs/3.2.3/reference/#_connecting_via_rest).

Damit Sie ein Gremlin-Script an den Endpunkt senden können, müssen Sie sich authentifizieren. Es gibt zwei Verfahren zur Authentifizierung für den Zugriff auf HTTP-Anforderungen: Basisauthentifizierung und Tokenauthentifizierung.

## Basisauthentifizierung

Bei der Basisauthentifizierung werden Ihr Benutzername und Ihr Kennwort mit der Anforderung gesendet. Dies ist bei einmaligen Abfragen angemessen, belastet jedoch bei mehreren Abfragen den Server aufgrund der Komplexität des Kennwortaustauschs und der Validierung. Bei curl können Sie den Benutzernamen und das Kennwort mit der Option `-u` übergeben:

```shell
$ curl -XPOST -d '{"gremlin" : "1+1" }' "https://portal91-0.ianuspater.compose-3.composedb.com:17916" -u admin:PASSWORDHERE
```

Sie können den Benutzernamen und das Kennwort auch in die Endpunkt-URL einbetten. 

## Tokenauthentifizierung

Für effizientere HTTP-Anforderungen können Sie ein Sitzungstoken von einem anderen Endpunkt abrufen. Dieses Token wird bei allen nachfolgenden Anforderungen an den Header übergeben. Wenn Sie mehrere Aufrufe für den HTTP-Endpunkt ausführen, können Sie mit der Tokenauthentifizierung die Leistung verbessern.

Verwenden Sie eine der Verbindungszeichenfolgen in der Anzeige _Sitzung_ der Übersichtsseite des Service-Dashboards. Die Verbindungszeichenfolgen enthalten den zum Abrufen des Tokens erforderlichen Aufruf bereits als curl-Befehl:

```shell
$ curl -XGET "https://admin:PASSWORDHERE@portal91-0.ianuspater.compose-3.composedb.com:17916/session"
```

Das zurückgegebene JSON-Ergebnis enthält das Sitzungstoken:

```
{
  "token": "YWRtaW46MTQ5NjkyODM1NDgzMzpWYXlxMERsMWs0VmltdGo3WUpzMEwtN3prUi00TGxZR3J6LXZnbDVmN3lnPQ=="
}
```

Das Token ist 60 Minuten gültig.
{: .tip}

Extrahieren Sie den Tokenwert und binden Sie ihn dann als Header für `Autorisierung` in alle weiteren Anforderungen ein:

```shell
$ curl -XPOST -H "Authorization: Token YWRtaW46MTQ5NjkyODM1NDgzMzpWYXlxMERsMWs0VmltdGo3WUpzMEwtN3prUi00TGxZR3J6LXZnbDVmN3lnPQ==" -d '{"gremlin" : "1+1" }' "https://portal91-0.ianuspater.compose-3.composedb.com:17916"
```

Wenn Sie das über die Shell tun und der Befehl [`jq`](https://stedolan.github.io/jq/) verfügbar ist, können Sie durch die Ausführung eines Befehls, der dem folgenden ähnelt, eine Umgebungsvariable festlegen:

```shell
$ export JGTOKEN=`curl -s -XGET "https://admin:PASSWORDHERE@portal91-0.ianuspater.compose-3.composedb.com:17916/session" | jq -r .token`
```

Führen Sie dann Anforderungen wie die folgende aus:

```shell
$ curl -XPOST -H "Authorization: Token $JGTOKEN" -d '{"gremlin" : "1+1" }' "https://portal91-0.ianuspater.compose-3.composedb.com:17916"
```

## HTTP JSON-Antworten

Antworten auf Abfragen erfolgen im Format eines JSON-Dokuments: mit einem Wert für die Anforderungs-ID, einem Statusobjekt und einem Ergebnisobjekt. Hier ein Beispiel für eine Antwort:

```json
{
  "requestId": "1bf4d1d4-eba4-484f-a94b-3cfa85df3448",
  "status": {
    "message": "",
    "code": 200,
    "attributes": {}
  },
  "result": {
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
}
```
`requestId` ist eine eindeutige ID für die Anforderung. `status` enthält eine lesbare Nachricht, einen Darstellungscode für den HTTP-Status und weitere Attribute des dokumentierten Status. `result` enthält ein Datenobjekt, das entsprechend der Ausgabe des ausgeführten Gremlin-Codes strukturiert ist, und einen Meta-Abschnitt für zusätzliche Informationen, die außerhalb des Bereichs der Ergebnisdaten liegen.

## WebSockets

WebSockets bieten ein effizientes Verfahren zum Erstellen einer bidirektionalen Verbindung mit langer Lebensdauer über TCP und TLS, das ideal für interaktive Sitzungen zwischen Clientanwendungen und dem JanusGraph-Server ist. Einige Bibliotheken für JanusGraph verwenden WebSockets als ihre Verbindung zum Server. Für die Arbeit mit JanusGraph on Compose müssen sie in der Lage sein, die Basisauthentifizierung und WSS (sichere WebSockets über TLS) zu verwenden. 

Sie können Ihre WebSockets-Verbindungszeichenfolgen aus dem Bereich _WebSocket_ der Übersichtsseite Ihres Service-Dashboards abrufen. Mit den URIs können Sie eine lange laufende Sitzung in der JanusGraph-Bereitstellung erstellen. Die URIs haben das Präfix `wss:`, das angibt, dass die Verbindungen mit HTTPS gesichert werden. Für diese Verbindungszeichenfolgen ist eine Basisauthentifizierung mithilfe der Berechtigungsnachweise des Benutzers mit Verwaltungsaufgaben erforderlich, damit eine Verbindung zum Server hergestellt werden kann.

## Gremlin-Konsole

Die Gremlin-Konsole ist ein wichtiges Tool für die Arbeit mit Tinkerpop-fähigen Servern. Die Gremlin-Konsole verwendet die WebSockets-Konnektivität, allerdings nicht die URI-Syntax zum Herstellen der Verbindung. Zum Teil liegt das daran, dass sie zum Arbeiten mit der Verbindung auch noch andere Elemente konfigurieren muss.

Zum Konfigurieren der Gremlin-Konsole stellen Sie eine YAML-Datei mit den entsprechenden Details bereit. Sie finden diese Konfiguration in der Anzeige _YAML-Datei für Gremlin-Konsole_ auf der Übersichtsseite des Service-Dashboards.

Weitere Informationen zum Abrufen, Installieren, Konfigurieren und Verwenden der Gremlin-Konsole finden Sie im Abschnitt [Diagramme mit der Gremlin-Konsole erstellen und traversieren](./tutorial-gremlin-console.html).
