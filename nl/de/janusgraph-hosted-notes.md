---
copyright:
  years: '2015, 2017'
lastupdated: '2017-11-21'
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Hinweise zum gehosteten JanusGraph-Service

{{site.data.keyword.composeForJanusGraph_full}} ist ein gehosteter Service, d. h. das Sicherstellen von sicheren Bereitstellungen unterscheidet sich in einigen Fällen vom JanusGraph-Service, der auf einem dedizierten Server ausgeführt wird. In diesem Thema werden diese Unterschiede und Variationen beschrieben. Die meisten dieser Unterschiede wirken sich auf die Art und Weise aus, wie die Gremlin-Sprachenabfragen erfolgen.

## Sandboxes

Um sicherzustellen, dass die Gremlin-Abfragen nicht auf Funktionen zugreifen, die das System beeinträchtigen könnten, werden alle Abfragen in einer Sandbox ausgeführt. Dies bedeutet, dass alle Funktions- und Methodenaufrufe eine Signatur haben müssen, die einem der folgenden regulären Ausdrücken entspricht:

```
java\.util.*
java\.lang\.Object.*
java\.lang\.Class <T extends java\.lang\.Object>#getSimpleName\(
java\.lang\.Iterable <T extends java\.lang\.Object>#iterator\(\)
java\.lang\.Boolean(?!#getBoolean\().*
java\.lang\.Double.*
java\.lang\.Float.*
java\.lang\.Integer(?!#getInteger\().*
java\.lang\.Long(?!#getLong\().*
java\.lang\.Math.*
java\.lang\.String(?!#intern\(\)).*
java\.lang\.StringBuilder.*
java\.lang\.Object#equals\(
java\.lang\.Object#hashCode\(
java\.util\.ArrayList#equals\(

org\.codehaus\.groovy\.runtime\.DefaultGroovyMethods.*
org\.codehaus\.groovy\.runtime\.StringGroovyMethods.*
org\.apache\.commons\.configuration\.MapConfiguration.*
org\.apache\.tinkerpop\.gremlin\.structure(?!\.io|\.Graph#toString\(|\.Graph#variables\(|\.Graph#configuration\(|\.Graph#io\(\)|\.util\.GraphFactory.*).*
org\.apache\.tinkerpop\.gremlin\.process\.traversal.*
org\.apache\.tinkerpop\.gremlin\.util\.Gremlin#version\(\)
org\.janusgraph\.core(?!\.JanusGraphFactory.*).*
org\.janusgraph\.graphdb(?!\.tinkerpop\.JanusGraphBlueprintsGraph#toString\(|\.tinkerpop\.JanusGraphBlueprintsGraph#variables\(|\.tinkerpop\.JanusGraphBlueprintsGraph#configuration\(|\.tinkerpop\.JanusGraphBlueprintsGraph#io\().*
org\.janusgraph\.example\.GraphOfTheGodsFactory(?!#create\(|#main\().*
org\.apache\.tinkerpop\.gremlin\.groovy\.plugin\.dsl\.credential\.CredentialGraph.*

Script\d+#.+\(?.+\)
groovy\.lang\.Closure <V extends java\.lang\.Object>#call\(?.+\)

org\.janusgraph\.util\.stats\.MetricManager#getRegistry\(\)
org\.janusgraph\.util\.stats\.MetricManager#INSTANCE
org\.apache\.tinkerpop\.gremlin\.util\.MetricManager#getRegistry\(\)
org\.apache\.tinkerpop\.gremlin\.util\.MetricManager#INSTANCE
```

Der Versuch, einen Funktions- oder Methodenaufruf auszuführen, der keinem der Einträge in der Whitelist der Funktionen entspricht, führt zu einem Fehler: 

```
[Static type checking] - Not authorized to call this method: ...
```

Die Sandbox erfordert, dass alle Variablen statisch deklariert sind, um die Sicherheitsprüfung zu ermöglichen. Dies kann mit dem Befehl `def` erreicht werden.

## Sitzungen

Eine Sitzung ermöglicht einer Verbindung zu {{site.data.keyword.composeForJanusGraph}}, den Status zwischen Anforderungen beizubehalten. Die meisten Optionen für den Verbindungsaufbau zu {{site.data.keyword.composeForJanusGraph}} haben keine zugeordneten Sitzungen. Das heißt, dass jedes über diese Verbindungsaufbaumethoden gesendete Gremlin-Script vollständig unabhängig sein muss. So müsste zum Beispiel das Öffnen eines Diagramms, mit dem das Script arbeiten muss, das Abfragen der entsprechenden Knoten und das Traversieren des Diagramms aus diesen Knoten in einer einzigen Anforderung verarbeitet werden. Diese Einschränkungen gelten sowohl für HTTP-Anforderungen als auch für WebSockets-Verbindungen. 

Eine Ausnahme ist, wenn Sie die Verbindung über die Gremlin-Konsole mithilfe des Befehls `:remote connect` herstellen.

```
:remote connect tinkerpop.server conf/compose.yaml
```

Wenn Sie die Verbindung mit dem oben angegebenen Befehl herstellen, gibt es keine Sitzung und die Verbindung wird auf dieselbe Weise wie HTTP-Anforderungen ausgeführt. Wenn Sie aber `session` als ein Argument hinzufügen, wird die Sitzungsunterstützung aktiviert. 

```
:remote connect tinkerpop.server conf/compose.yaml session
```

Bei Verwendung einer Sitzung können Sie in einem Befehl eine Variable definieren und in einem anderen Befehl auf sie verweisen.
{: .tip}

## Transaktionen

Alle Änderungen an der zugrunde liegenden JanusGraph-Datenbank sind in einer Transaktion eingebunden. Mit Transaktionen haben Sie die Möglichkeit, alle Änderungen innerhalb der Transaktionen festzuschreiben, um sie permanent zu speichern, oder rückgängig zu machen, um sicherzustellen, dass sie nicht gespeichert werden. 

Wenn Sie eine Anforderung über eine HTTP-Anforderung oder über einen WebSocket stellen, wird automatisch eine Transaktion gestartet, sobald Sie eine Änderung vornehmen, die in die Datenbank schreiben würde. Diese Transaktion wird außerdem automatisch festgeschrieben, wenn die Anforderung abgeschlossen ist.

Eine Ausnahme ist, wenn Sie die Verbindung über die Gremlin-Konsole mit aktivierter Sitzung herstellen. Die Sitzung kann eine lange Lebensdauer haben, sodass es beim Benutzer liegt, Änderungen durch das Aufrufen von `graph.tx().commit()` festzuschreiben. Hierbei ist `graph` das offene Diagramm, das die Änderungen enthält. Dies bedeutet zudem, dass Sie `graph.tx().rollback()` auch aufrufen können, um wieder zum Diagrammstatus vor dem Beginn der Transaktion zurückzugehen. 

## Gemischte Indizes

Die aktuelle Version von {{site.data.keyword.composeForJanusGraph}} unterstützt keine gemischten Indizes.
