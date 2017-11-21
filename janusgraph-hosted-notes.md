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

# JanusGraph Hosted Notes

{{site.data.keyword.composeForJanusGraph_full}} is a hosted service, so ensuring deployments are secure differs in a few cases from JanusGraph running on a dedicated server. This topic highlights those differences and variations. Most of these changes affect how the Gremlin language queries work.

## Sandboxes

To ensure that Gremlin queries do not access functionality that could compromise the system, all queries are run in a sandbox. This means all function and method calls must have a signature that matches one of the following regular expressions:

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

Trying to run a function or method call that does not match any of the entries in the whitelist of functions results in an error: 

```
[Static type checking] - Not authorized to call this method: ...
```

The sandbox also requires that all variables be statically declared to enable safety checking. You can do this with the `def` command.

## Sessions

A session allows a connection to {{site.data.keyword.composeForJanusGraph}} to maintain state between requests. Most connection options to {{site.data.keyword.composeForJanusGraph}} do not have sessions associated with them. That means that any Gremlin script sent through those connection methods needs to be entirely self-contained. For example, opening any graph the script needs to work with, querying for appropriate nodes, and traversing the graph from those nodes would all need to be handled in one request. This restriction applies to both HTTP requests and WebSockets connections. 

The exception to this is when you make connect from from the Gremlin console using `:remote connect`.

```
:remote connect tinkerpop.server conf/compose.yaml
```

If you make the connection using the command above there is no session, and the connection operates in the same way as HTTP requests. But if you add `session` as an argument,  then session support is now enabled.

```
:remote connect tinkerpop.server conf/compose.yaml session
```

Using a session, you can define a variable in one command and refer to it in another command.
{: .tip}

## Transactions

All changes to the underlying JanusGraph database are encapsulated in a transaction. Transactions allow any changes within them to be committed to make them permanent or rolled back to ensure they don't. 

When you make a request through an HTTP request or over a WebSocket, a transaction is automatically started when you make a change that would write to the database. That transaction is also automatically committed when the request is complete.

The exception is when you are connecting using the Gremlin Console with session enabled. The session can be long-lived, so it is up to the user to commit any changes by calling `graph.tx().commit()`, where `graph` is the open graph containing the tchanges. This also means you can call `graph.tx().rollback()` to go back to the graph's state before the transaction began. 

## Mixed Indexes

The current version of {{site.data.keyword.composeForJanusGraph}} does not support mixed indexes.
