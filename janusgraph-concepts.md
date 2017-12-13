---

copyright:
  years: 2017
lastupdated: "2017-12-12"
---

# JanusGraph Concepts

JanusGraph is a graph database. You can create, query and amend a number of graphs within the database. JanusGraph is built upon the Apache Tinkerpop stack and uses the Gremlin language for traversal, commands, and queries.

To read more about JanusGraph, see the [JanusGraph Documentation](http://docs.janusgraph.org/latest/index.html).

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="//www.youtube.com/embed/zTaoMWv6lnE?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

## Introduction to Graph Databases

At its simplest, a graph is a set of vertices with edges between them. The vertex, the singular form of vertices, is the fundamental unit of the graph and represents some indivisible object. In practical terms, this might be a person, location or object.  It can have properties to describe the object. 

An edge is a connection between two vertices expressing a relationship between them. It can have a multiplicity, direction, and properties.

In non-graph databases, relationships between entities are a secondary function of the database, expressed through shared keys in fields or created through JOINs. This means following relationships can require multiple queries or applications doing the work of assembling relationships from more general queries.

In graph databases, the relationship is a primary component of the data model and traversing from vertex to edge to vertex and beyond is the primary mechanism for querying the data within the model. That makes graph databases much more appropriate to queries that involve relationships such as "all the people who like band X and have a friend, or friend of a friend, who buys brand Y". 

Within JanusGraph, each graph has a unique name. To create, open, modify, and query a graph, you use the Gremlin language. A Gremlin query is composed of commands that can manipulate or traverse the graph in some way to provide the result you want.

## Vertices

A vertex is a graph element that represents an object. At vertex creation it has an identifier set by the graph engine and a label that is user defined. A vertex can additionally store properties which can be indexed and queried upon.

Read more about how to create, query, and other implementation details for vertices, see the [Tinkerpop/Gremlin documentation](http://tinkerpop.apache.org/docs/3.2.3/reference/#_the_graph_structure).

## Edges

An edge is a graph element that represents a relationship. To create an edge, the user provides the incoming/outgoing vertices and a label. The edge will be assigned an identifier by the graph engine. An edge can additionally store properties which can be indexed and queried upon.

Read more about how to create, query, and other implementation details for edges, see the [Tinkerpop/Gremlin documentation](http://tinkerpop.apache.org/docs/3.2.3/reference/#_the_graph_structure).

## Properties

A property is a key:value pair that describes, names, or is associated with an edge or vertex. Both edges and vertices may have multiple properties. For example, if the vertex is of type 'human', we can assign properties to that vertex of 'name', and 'age'.

Properties can be indexed so that vertices and edges can be retrieved by their properties, such as getting all the vertices with the same name.

Properties of edges may be things like 'weight', so that a graph traversal of vertices through multiple differently weighted edges can have a calculated total weight for the trip. 

## Traversal

Traversal is the process of analyzing a graph's structure. Traversal is the process discovers and returns information about edges, vertices, and their properties. To learn about the different Traversal steps and algorithms, see the [Tinkerpop/Gremlin documentation on traversal](http://tinkerpop.apache.org/docs/3.2.3/reference/#traversal).

## Gremlin

There are two parts to Gremlin: the language and the server.

Gremlin, the language, is a graph traversal language you use to interact and query your graphs.

Gremlin, the server, is a specification for a server which processes local or remote Gremlin language queries. An implementation of it is part of Apache Tinkerpop.

To perform a Gremlin language query, you send your Gremlin query to the appropriate Gremlin server. On Compose, that's a server running as part of your JanusGraph deployment.

## Apache TinkerPop

Apache TinkerPop is an open source Graph Computing Framework. TinkerPop is the system that models data as a graph and interacts with the Gremlin language commands to create, traverse, and manipulate graphs. To read more about how these pieces fit together, see the TinkerPop/Gremlin documentation on the [graph system integration](http://tinkerpop.apache.org/docs/3.2.3/reference/#_graph_system_integration).