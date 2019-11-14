---

copyright:
  years: 2017,2019
lastupdated: "2019-01-11"

keywords: janusgraph, compose, gremlin console

subcollection: compose-for-janusgraph

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Connecting to JanusGraph with the Gremlin Console
{: #gremlin-console}

The Gremlin Console is the essential tool for working with Tinkerpop enabled servers. Gremlin Console uses the WebSockets connectivity, but does not use the URI syntax for connecting. You need to configure it to work with the connection.

To configure Gremlin Console, you provide a YAML file with the appropriate details in it. You can find this configuration in the _Gremlin Console YAML_ panel on the service dashboard overview page.

## Installing Gremlin

To install the Gremlin Console,

1. Ensure that you have a recent version of Java installed.
2. Download [Gremlin Console version 3.3.3](https://archive.apache.org/dist/tinkerpop/3.3.3/apache-tinkerpop-gremlin-console-3.3.3-bin.zip).
3. Unzip the downloaded file to a working directory.
4. Using the command line or terminal, change directory into the root Gremlin Console directory and verify the download by executing `bin/gremlin.sh`:

  ```text
  $ unzip -q  ~/Downloads/apache-tinkerpop-gremlin-console-3.3.3-bin.zip
  $ cd apache-tinkerpop-gremlin-console-3.3.3
  $ bin/gremlin.sh

          \,,,/
          (o o)
  -----oOOo-(3)-oOOo-----
  plugin activated: tinkerpop.server
  plugin activated: tinkerpop.utilities
  plugin activated: tinkerpop.tinkergraph
  gremlin> [CONTROL-D]                                                             $

  ```

5. Configure the Gremlin Console. You can find your Gremlin Console YAML on the *Overview* page of your {{site.data.keyword.composeForJanusGraph}} service. Save one of the configurations to a file in the `conf` directory. In this example, we save it as `conf/compose.yaml`.
 
## Connecting

To verify the connection, execute `bin/gremlin.sh` again. Then, use the `:remote` command to connect to the tinkerpop server.

```text
:remote connect tinkerpop.server conf/compose.yaml session
```

This command tells Gremlin to `connect` to something that is a `tinkerpop.server` using the configuration in `conf/compose.yaml`. We're also passing another argument - `session` - to enable session support.

Type `:help :remote` to see the options for the `:remote` command.
{: tip}

If the connection is successful, you see an SSL warning and a message to confirm the connection.

```text
WARN  org.apache.tinkerpop.gremlin.driver.Cluster  - SSL configured without a trustCertChainFile and thus trusts all certificates without verification (not suitable for production)
==>Configured portal93-4.ianuspater.compose-3.composedb.com/159.8.153.216:18045-[2378d0de-0a93-4330-b8d8-848667f5b117]
gremlin>
```

## Sending Commands to the Server

The Gremlin console does not forward everything to the remote server automatically. To send a command to the server, you need to prefix the command with `:>`. Alternatively you can redirect all console commands to the remote server with `:remote console`.

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