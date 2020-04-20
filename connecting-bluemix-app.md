---

copyright:
  years: 2017,2019
lastupdated: "2019-01-11"

keywords: janusgraph, compose

subcollection: compose-for-janusgraph

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Connecting an {{site.data.keyword.cloud_notm}} application
{: #ibmcloud-cf-app}

To connect an {{site.data.keyword.cloud}} application to your service, use the service credentials that are created when the service is provisioned.

## Available credentials

Field Name|Description
----------|-----------
`deployment_id`|An internal identifier for the service as created within Compose.
`name`|The database deployment name.
`db_type`|The type of database that is offered by service; in this case `JanusGraph`.
`uri_cli`|A `CURL` command-line that illustrates how to send a gremlin query with token-based authentication.
`uri_cli_1`|A secondary `CURL` command-line that connects to the database instance.
`maps`|Unused
`uri`|The `https` URI that is used when connecting to the service, which includes the host name of server and port number to connect to.
`uri_direct_1`|A secondary `https` URI for connecting to the service.
`misc`|A parent field that contains the `session`, `websocket`, and `gremlin_console_yaml` fields.
`misc.session`| An `https` URI that one uses to retrieve a 60-minute authentication token that can be used to interact with the service. See [Connecting to JanusGraph with HTTP or WebSockets](/docs/ComposeForJanusGraph?topic=compose-for-janusgraph-http-websockets#token-authentication).
`misc.websocket`|The `wss` URIs that are used when connecting to the service using Websockets. See [Connecting to JanusGraph with HTTP or WebSockets](/docs/ComposeForJanusGraph?topic=compose-for-janusgraph-http-websockets#websockets).
`misc.gremlin_console_yaml`|YAML information that is used to configure the Gremlin console to connect to the service.  See [Connecting to JanusGraph with the Gremlin Console](/docs/ComposeForJanusGraph?topic=compose-for-janusgraph-gremlin-console).
{: caption="Table 1. Compose for JanusGraph credentials" caption-side="top"}

## Using the credentials

Your application needs to be connected to your service to use the service credentials. The following steps show how you can do this, using Node.js as an example.

### Connecting the application to the service

If you already have an {{site.data.keyword.cloud_notm}} application, you can connect it to a service from your {{site.data.keyword.cloud_notm}} account.

1. Log in to your {{site.data.keyword.cloud_notm}} account
2. From your dashboard, select the application that you want to connect to your service.
3. In the connections panel of the application _Overview_ page, click **Connect new** or **Connect existing** to connect to a new or existing {{site.data.keyword.cloud_notm}} service.

  - If you clicked **Connect existing**, a list of available services is shown. Select the service you want to connect your app to, and click **Connect**.
  - If you clicked **Connect new**, follow the steps to provision a new service instance. When you select a service from the catalogue to provision, the _Connect to_ field is automatically completed with the name of your application.

If you have an application that isn't uploaded to {{site.data.keyword.cloud_notm}} you can bind the application to the service before you upload it to {{site.data.keyword.cloud_notm}}: 

1. Log in to your {{site.data.keyword.cloud_notm}} account
2. Download and install the [Cloud Foundry CLI](https://github.com/cloudfoundry/cli) tool
3. Connect to {{site.data.keyword.cloud_notm}} in the command-line tool and follow the prompts to log in.

4. Open your application's `manifest.yml` file.
  - Change the `route` value to something unique. The route that you choose determines the subdomain of your application's URL: `<route>.{region}.cf.appdomain.cloud`. Be sure the `{region}` matches where your application is deployed.
  - Change the `name` value. The value that you choose is the name of the app as it appears in your {{site.data.keyword.cloud_notm}} dashboard.

5. Update the `services` value in `manifest.yml` to match the name of your service. `manifest.yml` will now look similar to

  ```
  ---
  applications:
  - name: compose-janusgraph-helloworld-nodejs
    routes:
      - route: compose-janusgraph-helloworld-nodejs.us-south.cf.appdomain.cloud
    memory:  128M
    services:
      - example-compose-for-janusgraph
```

### Accessing and using the credentials

Your application needs to parse the Cloud Foundry environvent variables. In Node.js, you can do this with the [cfenv](https://www.npmjs.com/package/cfenv) package.

1. Install the package with `--save` to update `package.json`:

  ```
  npm install --save cfenv
  ```

2. Extract the {{site.data.keyword.composeForJanusGraph_full}} credentials from the Cloud Foundry environment variables.

  ```javascript
  const cfenv = require('cfenv');
  const appenv = cfenv.getAppEnv();

  // Within the application environment (appenv) there's a services object
  const services = appenv.services;

  // The services object is a map named by service so extract the one for JanusGraph
  let janusgraph_services = services["compose-for-janusgraph"];

  // We now take the first bound JanusGraph service and extract its credentials object
  let credentials = janusgraph_services[0].credentials;
  ```

  You can now use the service credentials. For example, to get a session URI that you can use to obtain an authentication token:

  ```javascript
  let jgurl = credentials.misc.session[0];
  ```

3. Push the app to {{site.data.keyword.cloud_notm}}. When you push the app, it will automatically be bound to the service.

  ```
  $ cf push
  ```

For more information on using applications with {{site.data.keyword.cloud_notm}} services, see [Adding a service to your app](/docs/resources/connect_external_app?topic=resources-externalapp).