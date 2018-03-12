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

# Connecting to JanusGraph

JanusGraph supports connections using HTTP requests or over WebSockets. All connections are performed over HTTPS and require authentication. HTTP-based requests support basic and token authentication. For anything other than a one-off call on the JanusGraph deployment, using WebSockets or Token Authentication will result in better performance. Basic authentication is an expensive process that slows down requests.

For more on how to use HTTPs and to learn about creating and opening graphs, see [Creating and Traversing a Graph using HTTPS](./tutorial-https.html).
{: .tip}

The actual communication between client and database involves the client sending requests in Gremlin, a dialect of the Groovy language customized for querying graph structures and the database responding to those queries. You can also send requests using the Gremlin Console.

For details on how to get, install, configure and use the Gremlin Console, see [Creating and Traversing a Graph using Gremlin Console](./tutorial-gremlin-console.html).
{: .tip}

## HTTP requests

If you are using HTTP Requests to connect to the server, a single endpoint can be used to communicate with JanusGraph and is set up to round-robin across all available nodes in the deployment. It includes the host name and port.

You can get your HTTPS Connection Strings from your service dashboard overview page. The connection string requires basic authentication using the admin user credentials to connect to the server.

To connect to {{site.data.keyword.composeForJanusGraph}} with an HTTP request you need to use the HTTP POST method with the endpoint and send a JSON formatted document with a single entry with the key of "gremlin" and a value of the Gremlin query you wish to send. 

```json
{
    "gremlin": "a gremlin query would go here"
}
```

Gremlin is an extensive traversal language but a program in it can also be as simple as "1+1". To pass that as a script in a `curl` command the data part of a POST request is:

```
'{"gremlin" : "1+1" }'
``` 

If you're sending the same script to the server several times, you can improve performance by providing an optional "bindings" dictionary with a map of variables that are available to the gremlin script. This helps with performance as the server won't have to recompile the gremlin script.
{: .tip}

Additional information about the HTTP protocol can be found in the [Connecting via REST](http://tinkerpop.apache.org/docs/3.2.3/reference/#_connecting_via_rest) section of the Apache Tinkerpop Documentation

To send a Gremlin script to the endpoint, you need to be authenticated. There are two ways to be authenticated with HTTP request access: Basic authentication and token authentication.

## Basic Authentication

Basic authentication sends your username and password with the request. For one-off queries, it is reasonable, but any more than that and you begin loading the server with the complexity of doing the password exchange and validation. With curl you can pass the username and password with a `-u` option:

```shell
$ curl -XPOST -d '{"gremlin" : "1+1" }' "https://portal91-0.ianuspater.compose-3.composedb.com:17916" -u admin:PASSWORDHERE
```

You can also embed the username and password in the endpoint URL. 

## Token Authentication

For more efficient HTTP requests, you can obtain a session token from another endpoint. This token is passed in the header for all subsequent requests. If you are doing any more than one call on the HTTP endpoint, you can improve performance by using token authentication.

Use either of the connection strings in the _Session_ panel of the service dashboard overview page. The connection strings already include the call needed to obtain the token as a curl command:

```shell
$ curl -XGET "https://admin:PASSWORDHERE@portal91-0.ianuspater.compose-3.composedb.com:17916/session"
```

The returned JSON result contains the session token:

```
{
  "token": "YWRtaW46MTQ5NjkyODM1NDgzMzpWYXlxMERsMWs0VmltdGo3WUpzMEwtN3prUi00TGxZR3J6LXZnbDVmN3lnPQ=="
}
```

The token is valid for 60 minutes
{: .tip}

Extract the token value and then include it as the `Authorization` header in any further requests:

```shell
$ curl -XPOST -H "Authorization: Token YWRtaW46MTQ5NjkyODM1NDgzMzpWYXlxMERsMWs0VmltdGo3WUpzMEwtN3prUi00TGxZR3J6LXZnbDVmN3lnPQ==" -d '{"gremlin" : "1+1" }' "https://portal91-0.ianuspater.compose-3.composedb.com:17916"
```

If you are doing this from the shell and you have the [`jq`](https://stedolan.github.io/jq/) command available, then you can set an environment variable running a command like this:

```shell
$ export JGTOKEN=`curl -s -XGET "https://admin:PASSWORDHERE@portal91-0.ianuspater.compose-3.composedb.com:17916/session" | jq -r .token`
```

And then run requests like this:

```shell
$ curl -XPOST -H "Authorization: Token $JGTOKEN" -d '{"gremlin" : "1+1" }' "https://portal91-0.ianuspater.compose-3.composedb.com:17916"
```

## HTTP JSON responses

Responses to queries follow are in the form of a JSON document with a request id value, a status object, and a result object. Here is an example of a response:

```json
{
  "requestId": "1bf4d1d4-eba4-484f-a94b-3cfa85df3448",
  "status": {
    "message": "",
    "code": 200,
    "attributes": {}
  },
  "result": {
    "data": [
      {
        "god1": "hercules",
        "god2": "nemean"
      },
      {
        "god1": "hercules",
        "god2": "hydra"
      }
    ],
    "meta": {}
  }
}
```
The `requestId` is a unique identifier for the request. The `status` contains a readable message, a HTTP status style code and other attributes of the reported status. The `result` contains a data object that is structured according to whatever the executed Gremlin code returns and a meta section for any extra information that is outside the scope of the result data.

## Websockets

WebSockets are an efficient way of creating a long-lived two-way connection over TCP and TLS that is ideal for interactive sessions between client applications and the JanusGraph server. A number of libraries for JanusGraph use WebSockets as their connection to the server; to work with JanusGraph on Compose, they must be able to do basic authentication and to use WSS, secure WebSockets over TLS. 

You can get your Websockets Connection Strings from the _Websocket_ section of the service dashboard overview page. The URIs can be used to establish a long running session with the JanusGraph deployment, and are prefixed `wss:` to denote that the connections will be HTTPS secured. These connection strings require that basic authentication be used with the admin user credentials to connect to the server.

## Gremlin Console

The Gremlin Console is the essential tool for working with Tinkerpop enabled servers. Gremlin Console uses the WebSockets connectivity, but does not use the URI syntax for connecting; this is partly because it also needs to configure other elements to work with the connection.

To configure Gremlin Console you provide a YAML file with the appropriate details in it. You can find this configuration in the _Gremlin Console YAML_ panel on the service dashboard overview page.

For details on how to get, install, configure and use the Gremlin Console, see [Creating and Traversing a Graph using Gremlin Console](./tutorial-gremlin-console.html).