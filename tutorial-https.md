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

# Creating and Traversing a Graph using HTTPS

{{site.data.keyword.composeForJanusGraph_full}} comes with a sample graph database, 'The Graph of the Gods'. This tutorial uses that sample database to illustrate some JanusGraph concepts. Follow the steps to create, open and traverse the graph, using CURL to send Gremlin commands. To view a visual representation of the graph, see the JanusGraph [Getting Started](http://docs.janusgraph.org/latest/getting-started.html) documentation. 

## 1. Connect to Compose for JanusGraph

To connect to your {{site.data.keyword.composeForJanusGraph}} service using CURL you need to use a Connection String. The Connection String includes the user name, password, server and port number that is needed to connect to your service. You can find it on the _Overview_ page of your service dashboard.

Before you start the tutorial, verify that you can connect using the Connection String by passing a simple Gremlin command:

```
curl -XPOST -d '{"gremlin" : "1+1" }' "<CONNECTION STRING>"
```

If the request is successful, JanusGraph returns a response like this:

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

You are now ready to start working with The Graph of the Gods.

Add your Connection String on to the end of each of the CURL commands in the tutorial.
{: .tip}

## 2. Create the graph

{{site.data.keyword.composeForJanusGraph}} has a dedicated graph factory for creating, opening and closing graphs. Using this factory - `ConfiguredGraphFactory` - means you don't need to know about underlying storage mechanisms, so creating a new graph is just a matter of naming it. Start by creating a new graph, called _Example_.

The JanusGraph sandbox requires that you declare all variables you are using. To declare variables, you use the `def` keyword. For example, to declare the graph variable:

```
curl -XPOST -d '{"gremlin": "def graph=ConfiguredGraphFactory.create(\"example\");0;"}'
```

{{site.data.keyword.composeForJanusGraph}} graph names can include only alphanumeric characters and the underscore character.
{: .tip}

Note that the curl command ends with `;0;`. This is a workaround, because the HTTP request API emits an error (`{"message":"Cannot get namespace of root","Exception-Class":"java.lang.IllegalArgumentException"}`) even though it has completed the operation correctly. This only affects the graph type when code tries to return it.

## 3. Open the graph

Once you've created a graph, to work with a graph, you need to open it. You do this by calling `open()` on `JanusGraphConfiguredFactory`. Let's open our 'example' graph.

```
curl -XPOST -d '{"gremlin": "def graph=ConfiguredGraphFactory.open(\"example\");0;"}'
```

## 4. Load The Graph of the Gods

To load the sample graph, you call the `GraphOfTheGods.loadWithoutMixedIndex()` method, passing it your open 'example' graph, and the model is created for you.

```
curl -XPOST -d '{"gremlin": "def graph=ConfiguredGraphFactory.open(\"example\"); GraphOfTheGodsFactory.loadWithoutMixedIndex(graph,true);"}'
```

## 5. Prepare to traverse the graph

Now you have created and opened a graph, and loaded some data into it, you can explore the graph. To move around and query a graph, you need a traversal source, which is obtained by calling `traversal()` on the graph object. This means the full prefix for any command would be:

```
curl -XPOST -d '{"gremlin": "def graph=ConfiguredGraphFactory.open(\"example\");def g=graph.traversal();0;"}'
```

The use of `graph` for the graph and `g` for the traversal source is common. If you need only the traversal source, you can compress this down to:

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal();0;"}'
```

## 6. Traverse the graph to find Saturn

The Graph of the Gods graph contains a global index of name properties. This kind of index is often your first step in getting to a particular point in the graph. For example, to find "saturn" in the graph:

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next()"}'
```

This command returns the vertex that has the property `name: saturn`.

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

## 7. Traverse the graph to find the children of Saturn

To further the example, using the "saturn" vertex by defining it in a variable 'saturn', you can ask which vertices store an edge with the 'father' label pointing to it. Using the `in("father")` method selects those incoming edges, and using `values("name")` returns the value of the name property of that vertex.

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); ;g.V(saturn).in(\"father\").values(\"name\")"}'
```

The query returns the names of the children of Saturn in the graph, although there just the only child: "jupiter".

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

If you include the `in("father")` operation a second time, your traversal takes you to the Saturn vertex's grandchild.

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); ;g.V(saturn).in(\"father\").in(\"father\").values(\"name\")"}'
```

Using this query, you find that the name of Saturn's grandchild is 'hercules'.

## 8. Traverse the graph - Location

In the GraphOfTheGods, some edges have a "place" property, consisting of latitude and longitude. This property can be used for geolocating events. Edges labeled "battled" use the place property to represent where battles took place. To find all the battles that took place within 50 km of Athens (latitude: 37.97 and longitude: 23.72), search the edges of the graph for any value of the property "place" that falls within the radius of those coordinates.

You can select those edges using `has("place", geoWithin(Geoshape.circle(37.97, 23.72, 50)))`.

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); g.E().has(\"place\", geoWithin(Geoshape.circle(37.97, 23.72, 50)))"}'
```

The query returns the edge label for the incoming and outgoing vertices and their identifiers. To make the response more immediately readable, and to find out who participated in these two battles, we can use `as()` to stash the values returned as _god1_ and _god2_ and then `select()` to retrieve those values, using `by("name")` to extract the names of the two gods involved in each battle.

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

## 9. Traverse the graph - Vertices

In an earlier step you used two `in()` elements to identify that Hercules was the grandchild of Saturn. The traversal can also be expressed as a loop, because Gremlin can `repeat` a predicate:

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); def hercules=g.V(saturn).repeat(__.in(\"father\")).times(2).next()"}'
```

You can perform advanced operations that use the Hercules vertex as a starting point. For example, you can:

- Traverse the edges labeled "father" and "mother" and determine the "type" of parents Hercules had:

  ```
  curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); def hercules=g.V(saturn).repeat(__.in(\"father\")).times(2).next(); g.V(hercules).out(\"father\", \"mother\").label();" }'
  ```

- Traverse the "battled" edges to see who Hercules fought:

  ```
  curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); def hercules=g.V(saturn).repeat(__.in(\"father\")).times(2).next(); g.V(hercules).out(\"battled\").valueMap()" }' 
  ```

- Traverse the "battled" edges to see who Hercules fought more than once:

  ```
  curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); def hercules=g.V(saturn).repeat(__.in(\"father\")).times(2).next(); g.V(hercules).outE(\"battled\").has(\"time\", gt(1)).inV().values(\"name\")" }' 
  ```
  