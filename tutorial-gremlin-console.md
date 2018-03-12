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

# Creating and Traversing a Graph using Gremlin Console

This topic will help you get started with your first {{site.data.keyword.composeForJanusGraph_full}} database. For this tutorial we're going to use the Gremlin Console to send commands to the JanusGraph server.

## 1. Install Gremlin

To install the Gremlin Console:

1. Ensure you have a recent version of Java installed.
2. Download [Gremlin Console version 3.2.3](https://archive.apache.org/dist/tinkerpop/3.2.3/apache-tinkerpop-gremlin-console-3.2.3-bin.zip).
3. Unzip the downloaded file to a working directory.
4. Using the command line or terminal, change directory into the root Gremlin Console directory and verify the download by executing `bin/gremlin.sh`:

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

5. Configure the Gremlin Console. You can find your Gremlin Console YAML on the *Overview* page of your {{site.data.keyword.composeForJanusGraph}} service. Save one of the configurations to a file in the `conf` directory. In this example, we'll save it as `conf/compose.yaml`.
 
## 2. Connect to Compose for JanusGraph

To verify the connection, execute `bin/gremlin.sh` again. Then, use the `:remote` command to connect to the tinkerpop server:

```text
:remote connect tinkerpop.server conf/compose.yaml session
```

This command tells Gremlin to `connect` to something that is a `tinkerpop.server` using the configuration in `conf/compose.yaml`. We're also passing another argument - `session` - to enable session support.

Type `:help :remote` to see the options for the `:remote` command.
{: .tip}

If the connection is successful you'll see an SSL warning and a message to confirm the connection.

```text
WARN  org.apache.tinkerpop.gremlin.driver.Cluster  - SSL configured without a trustCertChainFile and thus trusts all certificates without verification (not suitable for production)
==>Configured portal93-4.ianuspater.compose-3.composedb.com/159.8.153.216:18045-[2378d0de-0a93-4330-b8d8-848667f5b117]
gremlin>
```

## 3. Send commands to the server.

The Gremlin console does not forward everything to the remote server automatically: to send a command to the server you need to prefix the command with `:>`. Alternatively you can redirect all console commands to the remote server with `:remote console`. This is demonstrated by the following series of commands and responses:

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

## 4. Create the graph

{{site.data.keyword.composeForJanusGraph}} has a dedicated graph factory for creating, opening and closing graphs. Using this factory - `ConfiguredGraphFactory` - means you don't need to know about underlying storage mechanisms, so creating a new graph is just a matter of naming it. Start by creating a new graph. In the sample commands in this tutorial, we're calling our graph _mygraph_.

Use the `def` keyword to declare the graph variable.

```
def graph=ConfiguredGraphFactory.create("mygraph")
```

{{site.data.keyword.composeForJanusGraph}} graph names can include only alphanumeric characters and the underscore character.
{: .tip}

## 5. Open the graph

Once you've created a graph, to work with a graph, you need to open it. You do this by calling `open()` on `ConfiguredGraphFactory`. Let's open our graph:

```
def graph=ConfiguredGraphFactory.open("mygraph")
```

At this point we need to commit the operation so that the graph persists:

```
graph.tx().commit()
```

If we close our session now, the new graph will be there when we come back.

## 6. Add vertices to the graph

Our graph will contain information on software developers and the pieces of software they have developed. Let's add a person called "marko" and a piece of software named "lop".

First, let's add marko.

```
def v1 = graph.addVertex(T.label, "person", "name", "marko", "age", 29)
```

If successful, this command will return the index of the vertex that we added - `==>v[4304]` - and we can go on to add the second vertex.

```
def v2 = graph.addVertex(T.label, "software", "name", "lop", "lang", "java")
```

Once again, we need to commit the operations.

```
graph.tx().commit()
```

## 7. Find a vertex in the graph

To find a vertex, say the one with the name "marko", we first traverse the graph and then use the has() method and it returns the identifier of the vertex.

```
def g=graph.traversal()
g.V().has("name","marko")
```

To reassign our vertices to the variables v1 and v2, say if we have closed our old session and started a new one, we use next().

```
def v1 = g.V().has("name","marko").next()
def v2 = g.V().has("name","lop").next()
```

We can then use these variables in subsequent Gremlin commands.

## 8. Add an edge

Next we can add a weighted edge between the two vertices, giving the edge a label of "created" and a weight of 0.4.

Note that you need to cast the number as a decimal, by specifying the value as `0.4d`.

```
v1.addEdge("created", v2, "weight", 0.4d)
```

Again, make sure you commit the change.

## 9. Query the graph

You can now query your graph to find the name of any software Marko has created.

```
g.V().has("name","marko").out("created").values("name")
```