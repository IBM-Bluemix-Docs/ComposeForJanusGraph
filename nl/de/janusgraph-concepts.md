---

copyright:
  years: 2017
lastupdated: "2017-08-04"
---

# JanusGraph - Konzepte

JanusGraph ist eine Diagrammdatenbank. Sie können eine Reihe von Diagrammen in der Datenbank erstellen, einrichten und erweitern. JanusGraph baut auf dem Apache Tinkerpop-Stapel auf und verwendet die Sprache Gremlin für die Traversierung, Befehle und Abfrage. Weitere Informationen zu JanusGraph finden Sie in der [Dokumentation zu JanusGraph](http://docs.janusgraph.org/latest/index.html).

## Einführung in Diagrammdatenbanken

Einfach ausgedrückt, ist ein Diagramm eine Gruppe von Vertices mit Kanten dazwischen. Der Vertex (die Einzahl von Vertices) ist die Grundeinheit des Diagramms und stellt bestimmte unteilbare Objekte dar. In der Praxis kann dies eine Person, eine Position oder ein Objekt sein. Das Objekt lässt sich mithilfe von Eigenschaften beschreiben. 

Eine Kante ist eine Verbindung zwischen zwei Vertices, die eine Beziehung zwischen diesen ausdrückt. Sie kann eine Multiplizität, eine Richtung und Eigenschaften haben.

In Nichtdiagrammdatenbanken sind Beziehungen zwischen Entitäten eine Sekundärfunktion der Datenbank, die mithilfe gemeinsam genutzter Schlüssel in Feldern ausgedrückt oder durch JOINs erstellt werden. Das bedeutet, dass für die folgenden Beziehungen mehrere Abfragen oder Anwendungen erforderlich sein können, die Beziehungen aus allgemeineren Abfragen assemblieren.

In Diagrammdatenbanken ist die Beziehung eine primäre Komponente des Datenmodells und das Traversieren vom Vertex zur Kante zum Vertex und darüber hinaus ist das primäre Verfahren zum Abfragen der Daten im Modell. Dadurch eignen sich Diagrammdatenbanken viel besser für Abfragen, in denen Beziehungen eine Rolle spielen. Beispiel: "Alle, die die Gruppe X mögen und einen Freund - oder den Freund einen Freundes - haben, der die Marke Y kauft". 

In JanusGraph hat jedes Diagramm einen eindeutigen Namen. Zum Erstellen, Öffnen und Abfragen eines Diagramms verwenden Sie die Sprache Gremlin. Eine Gremlin-Abfrage besteht aus Befehlen, die das Diagramm in einer bestimmten Weise bearbeiten oder traversieren können, um das gewünschte Ergebnis bereitzustellen.

## Vertices

Ein Vertex ist ein grafisches Element, das ein Objekt darstellt. Bei der Erstellung hat ein Vertex eine von der Diagrammengine festgelegte ID und eine benutzerdefinierte Bezeichnung. Ein Vertex kann außerdem Eigenschaften speichern, die indexiert und abgefragt werden können. Weitere Informationen zum Erstellen, Abfragen und zu anderen Implementierungsdetails von Vertices finden Sie in der [Dokumentation zu Tinkerpop/Gremlin](http://tinkerpop.apache.org/docs/3.2.3/reference/#_the_graph_structure).

## Kanten

Eine Kante ist ein grafisches Element, das eine Beziehung darstellt. Zum Erstellen einer Kante stellt der Benutzer die ein- und ausgehenden Vertices sowie eine Bezeichnung bereit. Die Diagrammengine weist der Kante eine ID zu. Eine Kante kann außerdem Eigenschaften speichern, die indexiert und abgefragt werden können. Weitere Informationen zum Erstellen, Abfragen und zu anderen Implementierungsdetails von Kanten finden Sie in der [Dokumentation zu Tinkerpop/Gremlin](http://tinkerpop.apache.org/docs/3.2.3/reference/#_the_graph_structure).

## Eigenschaften

Eine Eigenschaft ist ein Schlüssel/Wert-Paar, das Kanten oder Vertices beschreibt, benennt oder diesen zugeordnet ist. Sowohl Kanten als auch Vertices können mehrere Eigenschaften besitzen. Wenn der Vertex beispielsweise den Typ 'human' (Mensch) hat, kann man ihm die Eigenschaften 'name' (Name) und 'age' (Alter) zuweisen.

Eigenschaften können indexiert werden, sodass sich Vertices und Kanten anhand ihrer Eigenschaften abrufen lassen, beispielsweise alle Vertices mit demselben Namen.

Kanten können Eigenschaften wie 'weight' (Gewichtung) haben, sodass eine Diagrammtraversierung von Vertices durch mehrere Kanten mit unterschiedlicher Gewichtung eine berechnete Gesamtgewichtung für den Weg aufweisen kann. 

## Traversierung

Unter Traversierung versteht man den Prozess der Analyse der Struktur eines Diagramms. Eine Traversierung ist ein Vorgang, bei dem Informationen zu Kanten, Vertices und deren Eigenschaften erkannt und zurückgegeben werden. Weitere Informationen zu den verschiedenen Schritten und Algorithmen finden Sie im Abschnitt [Dokumentation zur Traversierung mit Tinkerpop/Gremlin](http://tinkerpop.apache.org/docs/3.2.3/reference/#traversal).

## Gremlin

Bei Gremlin gibt es zwei Teile: die Sprache und den Server.

Gremlin, die Sprache, ist eine Diagrammtraversierungssprache, mit der Sie Ihre Diagramme abfragen und mit ihnen interagieren können.

Gremlin, der Server, ist eine Spezifikation für einen Server, der lokale oder ferne Abfragen in der Sprache Gremlin verarbeitet. Eine Implementierung davon ist ein Teil von Apache Tinkerpop.

Um eine Abfrage in der Sprache Gremlin auszuführen, senden Sie Ihre Gremlin-Abfrage an den entsprechenden Gremlin-Server. Unter Compose ist das ein Server, der als Teil Ihrer JanusGraph-Bereitstellung ausgeführt wird.

## Apache TinkerPop

Apache TinkerPop ist ein Open-Source-Framework zur Datenverarbeitung in Diagrammen. TinkerPop ist das System, das Daten als Diagramm modelliert und durch die Interaktion mit den Befehlen der Sprache Gremlin Diagramme erstellt, traversiert und bearbeitet. Weitere Informationen dazu, wie diese Teile zusammenpassen, finden Sie in der Dokumentation zu TinkerPop/Gremlin im Abschnitt [Diagrammsystemintegration](http://tinkerpop.apache.org/docs/3.2.3/reference/#_graph_system_integration).
