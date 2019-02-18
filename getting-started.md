---

copyright:
  years: 2017, 2019
lastupdated: "2019-01-09"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Getting Started with JanusGraph
{: #getting-started}

This page is intended to help you get started with your first {{site.data.keyword.composeForJanusGraph_full}} database and Gremlin, JanusGraph's query language. It covers: 
1. Connecting with the Gremlin console or connecting with cURL
2. Creating, opening, listing, and deleting graphs in your database using `ConfiguredGraphFactory`
and 
3. Loading the 'The Graph of the Gods', so you can get familiar with graph databases and traversals.


## Getting Setup 

### With cURL

To connect to your {{site.data.keyword.composeForJanusGraph}} service using cURL, you need to use a Connection String. The Connection String includes the user name, password, server, and port number that is needed to connect to your service. You can find it on the _Overview_ page of your service dashboard.

Verify that you can connect by using the Connection String by passing a simple Gremlin command.

```
curl -XPOST -d '{"gremlin" : "1+1" }' "<CONNECTION STRING>"
```

If the request is successful, JanusGraph returns a response:

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

Add your Connection String on to the end of each of the CURL commands in the tutorial.
{: tip}

To learn more about connecting over HTTP and extending that to connect with WebSockets, see the [Connecting with HTTP and Websockets](/docs/services/ComposeForJanusGraph?topic=compose-for-janusgraph-http-websockets)

### With Gremlin Console

Using the Gremlin Console requires installation and setup. See the [Connecting with the Gremlin Console](/docs/services/ComposeForJanusGraph?topic=compose-for-janusgraph-gremlin-console) page for detailed instructions, if you need them.

Once you have installed the package and configured the YAML file, you can test your connection to your deployment by executing `bin/gremlin.sh`. Then, use the `:remote` command to connect to the tinkerpop server.

```text
:remote connect tinkerpop.server conf/compose.yaml session
```

This command tells Gremlin to `connect` to something that is a `tinkerpop.server` using the configuration in `conf/compose.yaml`. We're also passing another argument - `session` - to enable session support.

Type `:help :remote` to see the options for the `:remote` command.
{: tip}

If the connection is successful, an SSL warning and a message appear to confirm the connection.

```text
WARN  org.apache.tinkerpop.gremlin.driver.Cluster  - SSL configured without a trustCertChainFile and thus trusts all certificates without verification (not suitable for production)
==>Configured portal93-4.ianuspater.compose-3.composedb.com/159.8.153.216:18045-[2378d0de-0a93-4330-b8d8-848667f5b117]
gremlin>
```

## Using Configured Graph Factory

You can list all the graphs in your JanusGraph deployment with

```groovy
ConfiguredGraphFactory.getGraphNames();
```
Or 
```bash
curl -XPOST -d '{"gremlin": "ConfiguredGraphFactory.getGraphNames();}'
```

To create an "example" graph
```groovy
graph=ConfiguredGraphFactory.create("example");
```
Or
```bash
curl -XPOST -d '{"gremlin": "graph=ConfiguredGraphFactory.create(\"example\");0;"}'
```

{{site.data.keyword.composeForJanusGraph}} graph names can include only alphanumeric characters and the underscore character.
{: tip}

Note the curl command ends with `;0;`. This is a workaround, because the HTTP request API emits an error (`{"message":"Cannot get namespace of root","Exception-Class":"java.lang.IllegalArgumentException"}`) even though it has completed the operation correctly. This only affects the graph type when code tries to return it.

To open the example graph
```groovy
graph=ConfiguredGraphFactory.open("example");
```
Or
```bash
curl -XPOST -d '{"gremlin": "graph=ConfiguredGraphFactory.open(\"example\");0;"}'
```

And finally, to delete the example graph
```groovy
ConfiguredGraphFactory.drop("example")
```
Or
```bash
curl -XPOST -d '{"gremlin": "ConfiguredGraphFactory.drop(\"example");0;"}'
```

## Example: Graph of the Gods

As an example, {{site.data.keyword.composeForJanusGraph}} has a ready-made graph example that is called `GraphOfTheGods`, which can be loaded into a graph to traverse and explore. To use it, call the `GraphOfTheGods.loadWithoutMixedIndex()` method, passing it an open graph, and that model is created for you.
```groovy
graph=ConfiguredGraphFactory.create("example");
graph=ConfiguredGraphFactory.open("example");
GraphOfTheGodsFactory.loadWithoutMixedIndex(graph,true);
```
Or
```bash
curl -XPOST -d '{"gremlin": "graph=ConfiguredGraphFactory.create("example"); graph=ConfiguredGraphFactory.open(\"example\"); GraphOfTheGodsFactory.loadWithoutMixedIndex(graph,true);"}'
```

### Graph Traversal

Once you have created and opened the graph, you can traverse it using Gremlin. 
To get started, use
```groovy
graph=ConfiguredGraphFactory.open("example")
g=graph.traversal()
```
Or
```bash
curl -XPOST -d '{"gremlin": "graph=ConfiguredGraphFactory.open(\"example\"); g=graph.traversal();0;"}'
```

The use of `graph` for the graph and `g` for the traversal source is common.
{: tip}

Documentation and examples that use the Graph of the Gods are in the [JanusGraph Gremlin documentation](https://docs.janusgraph.org/latest/gremlin.html#_introductory_traversals).