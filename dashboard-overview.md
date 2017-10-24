---

Copyright:
  Years: 2017
lastupdated: "2017-09-05"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Service Overview

The _Overview_ page shows you information about your {{site.data.keyword.cloud_notm}} Compose database. The overview includes essential identifying information and current resource usage. You'll also find a section for connection strings that you can use with tools or make use of tools to connect to your database.

## Deployment Details

The _Deployment Details_ panel shows details of your service.

![Deployment Details](./images/janusgraph-deployment-details.png "A view of the Deployment Details panel")

### Type

The type of database that is offered by the service, and the database version that your service uses.

### Name

An internal identifier for the service.

### Usage

The size of your database and the amount of storage provided by your service plan.


## Connection Strings

Connection Strings can be used by some client libraries and contain all the information needed for other libraries to connect. You can find out how to use a Connection String to connect to your service in [Connecting an external application](./connecting-external.html).

You'll find each Connection String for your service in a different tab in the _Connection Strings_ panel.

### Session

You can use a session URI to get an authorization token that is valid for 60 minutes. You should use the provided token when making calls to the deployment using the HTTPS or Websocket URIs.

### HTTPS

This is the fundamental connection string for a JanusGraph deployment. To use an HTTPS connection string you need to provide the admin user credentials to connect to the server. For details on how to use HTTPs, see [Creating and Traversing a Graph using HTTPs](./tutorial-https.html).

### Websocket

The Websocket URIs can be used to establish a long-running session with the JanusGraph deployment. They are prefixed `wss:` to denote that the connections will be HTTPS secured. These connection strings require that basic authentication be used with the admin user credentials to connect to the server.

There are a number of libraries for JanusGraph that use WebSockets as their connection to the server; to work with {{site.data.keyword.composeForJanusGraph}} they must be able to do basic authentication and use WSS (secure WebSockets over TLS).

### Gremlin Console YAML

You can use either of the configurations provided to connect to {{site.data.keyword.composeForJanusGraph}} using the Gremlin Console. For details on how to use the Gremlin Console YAML, see [Creating and Traversing a Graph using Gremlin Console](./tutorial-gremlin-console.html).
