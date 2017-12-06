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

# Diagramme über HTTPS erstellen und traversieren

Im Lieferumfang von {{site.data.keyword.composeForJanusGraph_full}} ist eine Beispieldiagrammdatenbank namens 'The Graph of the Gods' enthalten. Diese Beispieldatenbank wird im vorliegenden Lernprogramm zur Veranschaulichung einiger JanusGraph-Konzepte verwendet. Führen Sie die Schritte zum Erstellen, Öffnen und Traversieren des Diagramms aus, wobei Sie Gremlin-Befehle mithilfe von CURL senden. Ein grafische Darstellung des Diagramms finden Sie in der Dokumentation zu JanusGraph im Abschnitt [Einführung](http://docs.janusgraph.org/latest/getting-started.html). 

## 1. Stellen Sie eine Verbindung zu Compose for JanusGraph her.

Um mithilfe von CURL eine Verbindung zu Ihrem {{site.data.keyword.composeForJanusGraph}}-Service herzustellen, müssen Sie eine Verbindungszeichenfolge verwenden. Die Verbindungszeichenfolge umfasst den Benutzernamen, das Kennwort, den Server und die Portnummer, die für die Verbindung zu Ihrem Service benötigt wird. Sie finden diese Angaben auf der Seite _Übersicht_ Ihres Service-Dashboards.

Stellen Sie vor dem Start des Lernprogramms sicher, dass Sie über die Verbindungszeichenfolge eine Verbindung herstellen können. Übergeben Sie dazu einen einfachen Gremlin-Befehl:

```
curl -XPOST -d '{"gremlin" : "1+1" }' "<CONNECTION STRING>"
```

Wenn die Anforderung erfolgreich ist, gibt JanusGraph eine Antwort wie die folgende zurück:

```
{
  requestId":"d2fe6ac7-95d9-4c79-a254-123648cad54d",
  "status":{
    "message":"",
    "code":200,
    "attributes":{}
  },
  "result":{
    "data":[2],
    "meta":{}
  }
}
```

Nun sind Sie startbereit für die Arbeit mit 'The Graph of the Gods'.

Fügen Sie Ihre Verbindungszeichenfolge jeweils an das Ende der einzelnen CURL-Befehle im Lernprogramm an.
{: .tip}

## 2. Erstellen Sie das Diagramm.

{{site.data.keyword.composeForJanusGraph}} besitzt eine dedizierte Diagrammfactory zum Erstellen, Öffnen und Schließen von Diagrammen. Wenn Sie diese Factory (`ConfiguredGraphFactory`) verwenden, müssen Sie die zugrunde liegenden Speichermechanismen nicht kennen. Dadurch beschränkt sich die Erstellung eines neuen Diagramms darauf, ihm einen Namen zu geben. Beginnen Sie mit der Erstellung eines neuen Diagramms namens _Example_.

Die JanusGraph-Sandbox verlangt, dass Sie alle Variablen deklarieren, die Sie verwenden. Zum Deklarieren der Variablen verwenden Sie das Schlüsselwort `def`. Gehen Sie beispielsweise zum Deklarieren der Variablen 'graph' wie folgt vor:

```
curl -XPOST -d '{"gremlin": "def graph=ConfiguredGraphFactory.create(\"example\");0;"}'
```

In {{site.data.keyword.composeForJanusGraph}} dürfen Diagrammnamen nur alphanumerische Zeichen und Unterstreichungszeichen enthalten.
{: .tip}

Beachten Sie, dass der curl-Befehl auf `;0;` endet. Das ist eine Ausweichlösung, weil die HTTP-Anforderungs-API einen Fehler ausgibt (`{"message":"Cannot get namespace of root","Exception-Class":"java.lang.IllegalArgumentException"}`), obwohl sie die Operation korrekt abgeschlossen hat. Dies wirkt sich nur dann auf den Diagrammtyp aus, wenn Code versucht, ihn zurückzugeben.

## 3. Öffnen Sie das Diagramm.

Nach der Erstellung eines Diagramms müssen Sie es öffnen, um damit arbeiten zu können. Dazu rufen Sie für `JanusGraphConfiguredFactory` die Methode `open()` auf. Das Diagramm 'example' wird wie folgt geöffnet.

```
curl -XPOST -d '{"gremlin": "def graph=ConfiguredGraphFactory.open(\"example\");0;"}'
```

## 4. Laden Sie 'The Graph of the Gods'.

Um das Beispieldiagramm zu laden, müssen Sie die Methode `GraphOfTheGods.loadWithoutMixedIndex()` aufrufen und sie an Ihr geöffnetes Diagramm 'example' übergeben. Daraufhin wird das Modell für Sie geöffnet.

```
curl -XPOST -d '{"gremlin": "def graph=ConfiguredGraphFactory.open(\"example\"); GraphOfTheGodsFactory.loadWithoutMixedIndex(graph,true);"}'
```

## 5. Bereiten Sie die Traversierung des Diagramms vor.

Nachdem Sie nun ein Diagramm erstellt und geöffnet und Daten hineingeladen haben, können Sie es untersuchen. Um in einem Diagramm zu blättern und es abzufragen, benötigen Sie eine Traversierungsquelle, die durch den Aufruf von `traversal()` für das Diagrammobjekt abgerufen wird. Das vollständige Präfix jedes Befehls lautet somit wie folgt:

```
curl -XPOST -d '{"gremlin": "def graph=ConfiguredGraphFactory.open(\"example\");def g=graph.traversal();0;"}'
```

Es ist üblich, `graph` für das Diagramm und `g` für die Traversierungsquelle zu verwenden. Wenn Sie nur die Traversierungsquelle benötigen, lässt sich der Befehl wie folgt komprimieren:

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal();0;"}'
```

## 6. Traversieren Sie das Diagramm, um Saturn zu suchen.

Das Diagramm 'The Graph of the Gods' enthält einen globalen Index mit Namenseigenschaften. Diese Art Index stellt oft Ihren ersten Schritt zum Erreichen eines bestimmten Punkts im Diagramm dar. Gehen Sie beispielsweise wie folgt vor, um im Diagramm "saturn" zu suchen:

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next()"}'
```

Dieser Befehl gibt den Vertex mit der Eigenschaft `name: saturn` zurück.

```
{
  "requestId":"34d349a7-fa8e-4268-8b8d-a8adc0056bf3",
  "status":{
    "message":"",
    "code":200,
    "attributes":{}
  },
  "result":{
    "data":[
      {
        "id":4184,
        "label":"titan",
        "type":"vertex",
        "properties":{
          "name":[
            {
              "id":"16z-388-sl",
              "value":"saturn"
            }
          ],
          "age":[
            {
              "id":"1l7-388-35x",
              "value":10000
            }
          ]
        }
      }
    ],
  "meta":{}
  }
}
```

## 7. Traversieren Sie das Diagramm, um die untergeordneten Elemente von Saturn zu suchen.

Sie können das Beispiel fortführen, indem Sie den Vertex "saturn" in einer Variablen namens 'saturn' definieren und mit diesem Vertex abfragen, in welchen Vertices eine Kante gespeichert ist, auf die die Bezeichnung 'father' verweist. Durch die Verwendung der Methode `in("father")` werden diese eingehenden Kanten ausgewählt, während mit `values("name")` der Wert der Namenseigenschaft dieses Vertex zurückgegeben wird.

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); ;g.V(saturn).in(\"father\").values(\"name\")"}'
```

Die Abfrage gibt die Namen der Elemente im Diagramm zurück, die Saturn untergeordnet sind, auch wenn nur ein einziges untergeordnetes Element vorhanden ist: "jupiter".

```
{
  "requestId":"2ad6c01e-f724-4e0a-8895-4e8e368fb7ca",
  "status":{
    "message":"",
    "code":200,
    "attributes":{}
  },
  "result":{
    "data":[
      "jupiter"
    ],
    "meta":{}
  }
}
```

Wenn Sie die Operation `in("father")` noch einmal einbinden, bringt Ihre Traversierung Sie zum Enkelelement des Vertex Saturn.

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); ;g.V(saturn).in(\"father\").in(\"father\").values(\"name\")"}'
```

Mit dieser Abfrage finden Sie heraus, dass das Enkelelement von Saturn den Namen 'hercules' hat.

## 8. Traversieren Sie das Diagramm - Position.

In GraphOfTheGods besitzen einige Kanten eine Eigenschaft namens "place", die aus einem Breitengrad und einem Längengrad besteht. Diese Eigenschaft lässt sich zur geografischen Positionierung von Ereignissen verwenden. Kanten mit der Bezeichnung "battled" stellen mit der Eigenschaft "place" den Ort von Schlachten dar. Um alle Schlachten zu finden, die innerhalb von 50 km um Athen stattgefunden haben (Breitengrad: 37,97 und Längengrad: 23,72), müssen Sie in den Kanten des Diagramms nach allen Werten der Eigenschaft "place" suchen, die sich im Radius dieser Koordinaten befinden.

Sie können diese Kanten mit dem folgenden Befehl auswählen: `has("place", geoWithin(Geoshape.circle(37.97, 23.72, 50)))`.

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); g.E().has(\"place\", geoWithin(Geoshape.circle(37.97, 23.72, 50)))"}'
```

Die Abfrage gibt die Kantenbezeichnung für die ein- und ausgehenden Vertices sowie deren IDs zurück. Um die Antwort schneller lesbar zu machen und zu ermitteln, wer an diesen beiden Schlachten beteiligt war, können Sie die zurückgegebenen Werte mit `as()` als _god1_ und _god2_ verdeckt speichern und diese Werte mit `select()` abrufen, wobei die Namen der beiden Götter, die an den einzelnen Schlachten beteiligt waren, mit `by("name")` extrahiert werden.

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); g.E().has(\"place\", geoWithin(Geoshape.circle(37.97, 23.72, 50))).as(\"source\").inV().as(\"god2\").select(\"source\").outV().as(\"god1\").select(\"god1\", \"god2\").by(\"name\")"}'
```

```
"result":{
  "data":[
    {
      "god1":"hercules",
      "god2":"nemean"
    },
    {
      "god1":"hercules",
      "god2":"hydra"
    }
  ],
  "meta":{}
}
```

## 9. Traversieren Sie das Diagramm - Vertices.

In einem früheren Schritt haben Sie mithilfe von zwei `in()`-Elementen ermittelt, dass Hercules das Enkelelement von Saturn war. Die Traversierung kann auch als Schleife ausgedrückt werden, weil Gremlin mit `repeat` ein Prädikat wiederholen kann:

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); def hercules=g.V(saturn).repeat(__.in(\"father\")).times(2).next()"}'
```

Sie können erweiterte Operationen durchführen, die den Vertex Hercules als Ausgangspunkt verwenden. Sie haben beispielsweise folgende Möglichkeiten:

- Traversieren Sie die Kanten mit der Bezeichnung "father" und "mother" und bestimmen Sie den "type" (Typ) der Eltern von Hercules:

  ```
  curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); def hercules=g.V(saturn).repeat(__.in(\"father\")).times(2).next(); g.V(hercules).out(\"father\", \"mother\").label();" }'
  ```

- Traversieren Sie die Kante mit der Bezeichnung "battled", um zu sehen, wo Hercules gekämpft hat:

  ```
  curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); def hercules=g.V(saturn).repeat(__.in(\"father\")).times(2).next(); g.V(hercules).out(\"battled\").valueMap()" }' 
  ```

- Traversieren Sie die Kante mit der Bezeichnung "battled", um zu sehen, wo Hercules mehrmals gekämpft hat:

  ```
  curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); def hercules=g.V(saturn).repeat(__.in(\"father\")).times(2).next(); g.V(hercules).outE(\"battled\").has(\"time\", gt(1)).inV().values(\"name\")" }'
  ```
  
