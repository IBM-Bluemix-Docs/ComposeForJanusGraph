---

copyright:
  years: 2017
lastupdated: "2017-09-13"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Diagramme über die Gremlin-Konsole erstellen und traversieren

Dieser Abschnitt hilft Ihnen bei den ersten Schritten mit Ihrer ersten {{site.data.keyword.composeForJanusGraph_full}}-Datenbank. In diesem Lernprogramm wird die Gremlin-Konsole dazu verwendet, Befehle an den JanusGraph-Server zu senden.

## 1. Installieren Sie Gremlin.

Gehen Sie wie folgt vor, um die Gremlin-Konsole zu installieren:

1. Stellen Sie sicher, dass die neueste Java-Version installiert ist.
2. Laden Sie die [Gremlin-Konsole Version 3.2.3](https://archive.apache.org/dist/tinkerpop/3.2.3/apache-tinkerpop-gremlin-console-3.2.3-bin.zip) herunter.
3. Dekomprimieren Sie die heruntergeladene Datei in ein Arbeitsverzeichnis.
4. Wechseln Sie über die Befehlszeile oder das Terminal in das Stammverzeichnis der Gremlin-Konsole und prüfen Sie den Download mit dem Befehl `bin/gremlin.sh`:

  ```text
  $ unzip -q  ~/Downloads/apache-tinkerpop-gremlin-console-3.2.3-bin.zip
  $ cd apache-tinkerpop-gremlin-console-3.2.3
  $ bin/gremlin.sh

          \,,,/
          (o o)
  -----oOOo-(3)-oOOo-----
  plugin activated: tinkerpop.server
  plugin activated: tinkerpop.utilities
  plugin activated: tinkerpop.tinkergraph
  gremlin> [CONTROL-D]                                                             $

  ```

5. Konfigurieren Sie die Gremlin-Konsole. Sie finden die YAML-Datei für Ihre Gremlin-Konsole auf der Seite *Übersicht* Ihres {{site.data.keyword.composeForJanusGraph}}-Service. Speichern Sie eine der Konfigurationen in einer Datei im Verzeichnis `conf`. Im vorliegenden Beispiel wird wird sie als `conf/compose.yaml` gespeichert.
 
## 2. Stellen Sie eine Verbindung zu Compose for JanusGraph her.

Führen Sie zur Überprüfung der Verbindung `bin/gremlin.sh` noch einmal aus. Stellen Sie dann mit dem Befehl `:remote` eine Verbindung zum Tinkerpop-Server her:

```text
:remote connect tinkerpop.server conf/compose.yaml session
```

Dieser Befehl weist Gremlin an, mithilfe der Konfiguration in `conf/compose.yaml` per `connect` eine Verbindung zu einem als `tinkerpop.server` bezeichneten Element herzustellen. Außerdem wird ein weiteres Argument - `session` - übergeben, um die Sitzungsunterstützung zu aktivieren.

Geben Sie `:help :remote` ein, um die Optionen für den Befehl `:remote` anzuzeigen.
{: .tip}

Wenn der Verbindungsaufbau erfolgreich ist, werden eine SSL-Warnung und eine Nachricht zur Bestätigung der Verbindung angezeigt.

```text
WARN  org.apache.tinkerpop.gremlin.driver.Cluster  - SSL configured without a trustCertChainFile and thus trusts all certificates without verification (not suitable for production)
==>Configured portal93-4.ianuspater.compose-3.composedb.com/159.8.153.216:18045-[2378d0de-0a93-4330-b8d8-848667f5b117]
gremlin>
```

## 3. Senden Sie Befehle an den Server.

Die Gremlin-Konsole leitet nicht alles automatisch an den fernen Server weiter: Um einen Befehl an den Server zu senden, müssen Sie dem Befehl das Präfix `:>` voranstellen. Alternativ können Sie alle Konsolenbefehle mit `:remote console` an den fernen Server weiterleiten. Dies veranschaulichen die folgenden Befehle und Antworten:

```text
$ bin/gremlin.sh

        \,,,/
        (o o)
-----oOOo-(3)-oOOo-----
plugin activated: tinkerpop.server
plugin activated: tinkerpop.utilities
plugin activated: tinkerpop.tinkergraph
gremlin> 1+1
==>2
gremlin> :> 1+1
==>No remotes are configured.  Use :remote command.
gremlin> :remote connect tinkerpop.server conf/compose.yaml session
WARN  org.apache.tinkerpop.gremlin.driver.Cluster  - SSL configured without a trustCertChainFile and thus trusts all certificates without verification (not suitable for production)
==>Configured portal93-4.ianuspater.compose-3.composedb.com/159.8.153.216:18045-[016d7a68-cf70-450e-92eb-4e5e2a647b5b]
gremlin> :remote console
==>All scripts will now be sent to Gremlin Server - [portal93-4.ianuspater.compose-3.composedb.com/159.8.153.216:18045]-[016d7a68-cf70-450e-92eb-4e5e2a647b5b] - type ':remote console' to return to local mode
gremlin> 1+1
==>2
gremlin>

```

## 4. Erstellen Sie das Diagramm.

{{site.data.keyword.composeForJanusGraph}} besitzt eine dedizierte Diagrammfactory zum Erstellen, Öffnen und Schließen von Diagrammen. Wenn Sie diese Factory (`ConfiguredGraphFactory`) verwenden, müssen Sie die zugrunde liegenden Speichermechanismen nicht kennen. Dadurch beschränkt sich die Erstellung eines neuen Diagramms darauf, ihm einen Namen zu geben. Beginnen Sie mit der Erstellung eines neuen Diagramms. In den Beispielbefehlen dieses Lernprogramms hat das Diagramm den Namen _mygraph_.

Deklarieren Sie die Diagrammvariable mit dem Schlüsselwort `def`.

```
def graph=ConfiguredGraphFactory.create("mygraph")
```

In {{site.data.keyword.composeForJanusGraph}} dürfen Diagrammnamen nur alphanumerische Zeichen und Unterstreichungszeichen enthalten.
{: .tip}

## 5. Öffnen Sie das Diagramm.

Nach der Erstellung eines Diagramms müssen Sie es öffnen, um damit arbeiten zu können. Dazu rufen Sie für `ConfiguredGraphFactory` die Methode `open()` auf. Das Diagramm wird wie folgt geöffnet:

```
def graph=ConfiguredGraphFactory.open("mygraph")
```

Nun muss die Operation festgeschrieben werden, damit das Diagramm  bestehen bleibt.

```
graph.tx().commit()
```

Wenn die Sitzung nun geschlossen wird, ist das neue Diagramm vorhanden, wenn sie wieder geöffnet wird.

## 6. Fügen Sie Vertices zum Diagramm hinzu.

Das Diagramm soll Informationen zu Softwareentwicklern und der von ihnen jeweils entwickelten Software enthalten. Fügen Sie also eine Person namens "marko" und eine Software namens "lop" hinzu.

Fügen Sie zuerst "marko" hinzu.

```
def v1 = graph.addVertex(T.label, "person", "name", "marko", "age", 29)
```

Wenn der Befehl erfolgreich war, gibt er den Index des hinzugefügten Vertex - `==>v[4304]` - zurück. Nun kann der zweite Vertex hinzugefügt werden.

```
def v2 = graph.addVertex(T.label, "software", "name", "lop", "lang", "java")
```

Die Operationen müssen wieder festgeschrieben werden.

```
graph.tx().commit()
```

## 7. Suchen Sie einen Vertex im Diagramm.

Um einen Vertex zu suchen, beispielsweise den Namen "marko", traversieren Sie zuerst das Diagramm und führen dann die Methode has() aus, die die ID des Vertex zurückgibt.

```
def g=graph.traversal()
g.V().has("name","marko")
```

Um die Vertices erneut den Variablen v1 und v2 zuzuordnen, wenn beispielsweise die alte Sitzung geschlossen und eine neue geöffnet wurde, verwenden Sie next().

```
def v1 = g.V().has("name","marko").next()
def v2 = g.V().has("name","lop").next()
```

Sie können diese Variablen anschließend in den nachfolgenden Gremlin-Befehlen verwenden.

## 8. Fügen Sie eine Kante hinzu.

Als Nächstes können Sie eine gewichtete Kante mit der Bezeichnung "created" (erstellt) und einer Gewichtung von 0,4 zwischen den beiden Vertices hinzufügen.

Beachten Sie, dass Sie die Zahl als Dezimalzahl umsetzen und dazu den Wert `0.4d` angeben müssen.

```
v1.addEdge("created", v2, "weight", 0.4d)
```

Achten Sie wieder darauf, die Änderung festzuschreiben.

## 9. Fragen Sie das Diagramm ab.

Nun können Sie Ihr Diagramm abfragen und nach dem Namen der Software suchen, die Marko erstellt hat.

```
g.V().has("name","marko").out("created").values("name")
```
