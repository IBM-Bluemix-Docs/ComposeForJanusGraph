---

copyright:
  years: 2017,2018
lastupdated: "2018-05-08"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# {{site.data.keyword.cloud_notm}}-Anwendung verbinden

Verbinden Sie eine {{site.data.keyword.cloud}}-Anwendung mit Ihrem Service mithilfe der Berechtigungsnachweise dieses Service, die bei dessen Bereitstellung erstellt werden.

## Verfügbare Berechtigungsnachweise

Feldname|Beschreibung
----------|-----------
`deployment_id`|Eine interne ID für den Service entsprechend der Erstellung in Compose.
`name`|Der Name der Datenbankimplementierung.
`db_type`|Der Datenbanktyp, der vom Service angeboten wird, in diesem Fall `JanusGraph`.
`uri_cli`|Eine `CURL`-Befehlszeile, die veranschaulicht, wie eine Gremlin-Abfrage mit tokenbasierter Authentifizierung gesendet wird.
`uri_cli_1`|Eine sekundäre `CURL`-Befehlszeile, mit der eine Verbindung zur Datenbankinstanz hergestellt wird.
`maps`|Nicht verwendet
`uri`|Der bei der Herstellung einer Verbindung zum Service verwendete `https`-URI, der den Hostnamen des Servers und die Portnummer für die Verbindungsherstellung enthält. Weitere Informationen finden Sie im Abschnitt [Externe Anwendung verbinden](./connecting-external.html).
`uri_direct_1`|Ein sekundärer `https`-URI zum Verbinden mit dem Service.
`misc`|Ein übergeordnetes Feld, das die Felder `session`, `websocket` und `gremlin_console_yaml` enthält.
`misc.session`| Ein `https`-URI zum Abrufen eines 60-minütigen Authentifizierungstokens, das zur Interaktion mit dem Service verwendet werden kann. Weitere Informationen finden Sie im Abschnitt [Externe Anwendung verbinden - Tokenauthentifizierung](./connecting-external.html#token-authentication).
`misc.websocket`|Die `wss`-URIs, die beim Herstellen einer Verbindung zu dem Service mithilfe von WebSockets verwendet werden. Weitere Informationen finden Sie im Abschnitt [Externe Anwendung verbinden - WebSockets](./connecting-external.html#websockets).
`misc.gremlin_console_yaml`|YAML-Informationen, mit denen die Gremlin-Konsole zum Herstellen einer Verbindung zu dem Service konfiguriert wird.  Weitere Informationen finden Sie im Abschnitt [Externe Anwendung verbinden - Gremlin-Konsole ](./connecting-external.html#gremlin-console).
{: caption="Tabelle 1. Compose for JanusGraph - Berechtigungsnachweise" caption-side="top"}

## Berechtigungsnachweise verwenden

Um die Berechtigungsnachweise Ihres Service verwenden zu können, muss Ihre Anwendung mit dem Service verbunden sein. Mit den folgenden Schritten wird anhand des Beispiels Node.js gezeigt, wie Sie das tun können.

### Verbindung von der Anwendung zum Service herstellen

Wenn Sie bereits über eine {{site.data.keyword.cloud_notm}}-Anwendung verfügen, können Sie sie über Ihr {{site.data.keyword.cloud_notm}}-Konto mit einem Service verbinden:

1. Melden Sie sich an Ihrem {{site.data.keyword.cloud_notm}}-Konto an.
2. Wählen Sie in Ihrem Dashboard die Anwendung aus, die Sie mit Ihrem Service verbinden wollen.
3. Klicken Sie in der Verbindungsanzeige der Seite _Übersicht_ der Anwendung auf **Neuen verbinden** oder **Vorhandenen verbinden**, um eine Verbindung zu einem neuen oder vorhandenen {{site.data.keyword.cloud_notm}}-Service herzustellen.

  - Wenn Sie auf **Vorhandenen verbinden** geklickt haben, wird eine Liste der verfügbaren Services angezeigt. Wählen Sie den Service aus, mit dem Sie Ihre App verbinden wollen, und klicken Sie auf **Verbinden**.
  - Wenn Sie auf **Neuen verbinden** klicken, müssen Sie die Schritte zum Bereitstellen einer neuen Serviceinstanz ausführen. Wenn Sie einen Service aus dem Katalog zur Bereitstellung auswählen, wird im Feld _Verbindung herstellen zu_ automatisch der Name Ihrer Anwendung eingetragen.

Wenn Sie eine Anwendung haben, die Sie nicht auf {{site.data.keyword.cloud_notm}} hochgeladen haben, können Sie sie vor dem Hochladen auf {{site.data.keyword.cloud_notm}} an den Service binden: 

1. Melden Sie sich an Ihrem {{site.data.keyword.cloud_notm}}-Konto an.
2. Laden Sie das Tool [Cloud Foundry CLI](https://github.com/cloudfoundry/cli) herunter und installieren Sie es.
3. Stellen Sie im Befehlszeilentool eine Verbindung zu {{site.data.keyword.cloud_notm}} her und befolgen Sie die Eingabeaufforderungen zur Anmeldung.

  ```
  $ cf api https://api.ng.bluemix.net
  $ cf login
  ```

4. Öffnen Sie die Datei `manifest.yml` Ihrer Anwendung.

  - Ändern Sie den Wert für `Host` in einen eindeutigen Wert. Der gewählte Host bestimmt die Unterdomäne Ihrer Anwendungs-URL: `<host>.mybluemix.net`.
  - Ändern Sie den Wert für `Name`. Der gewählte Wert wird als Name der App in Ihrem {{site.data.keyword.cloud_notm}}-Dashboard angezeigt.

5. Aktualisieren Sie den Wert für `Services` in `manifest.yml`, sodass er mit dem Namen Ihres Service übereinstimmt. `manifest.yml` sieht ähnlich aus wie das folgende Beispiel:

  ```
  ---
  applications:
  - name:    compose-janusgraph-sample-service
    host:    compose-janusgraph-sample-service
    memory:  256M
    services:
      - compose-janusgraph-sample-app
  ```

### Auf Berechtigungsnachweise zugreifen und sie verwenden

Ihre Anwendung muss die Cloud Foundry-Umgebungsvariablen parsen. In Node.js können Sie das mit dem Paket [cfenv](https://www.npmjs.com/package/cfenv) tun.

1. Installieren Sie das Paket `--save`, um `package.json` zu aktualisieren:

  ```
  npm install --save cfenv
  ```

2. Extrahieren Sie die Berechtigungsnachweise von {{site.data.keyword.composeForJanusGraph_full}} aus den Cloud Foundry-Umgebungsvariablen.

  ```javascript
  const cfenv = require('cfenv');
  const appenv = cfenv.getAppEnv();

  // Within the application environment (appenv) there's a services object
  const services = appenv.services;

  // The services object is a map named by service so extract the one for JanusGraph
  let janusgraph_services = services["compose-for-janusgraph"];

  // We now take the first bound JanusGraph service and extract its credentials object
  let credentials = janusgraph_services[0].credentials;
  ```

  Nun können Sie die Berechtigungsnachweise des Service verwenden. Beispielsweise, um einen Sitzungs-URI abzurufen, mit dem Sie ein Authentifizierungstoken abrufen können:

  ```javascript
  let jgurl = credentials.misc.session[0];
  ```

3. Übertragen Sie die App mit einer Push-Operation auf {{site.data.keyword.cloud_notm}}. Bei der Push-Operation wird die App automatisch an den Service gebunden.

  ```
  $ cf push
  ```

Weitere Informationen zur Verwendung von Anwendungen mit {{site.data.keyword.cloud_notm}}-Services finden Sie im Abschnitt [Service zur App hinzufügen](https://console.{DomainName}/docs/services/reqnsi.html#add_service).
