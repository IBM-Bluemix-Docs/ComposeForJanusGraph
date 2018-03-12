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

# Connecting an {{site.data.keyword.cloud_notm}} application

To connect an {{site.data.keyword.cloud}} application to your service, use the service credentials that are created when the service is provisioned.

## Available credentials

Field Name|Description
----------|-----------
`deployment_id`|An internal identifier for the service as created within Compose.
`name`|The database deployment name.
`db_type`|The type of database that is offered by service; in this case `JanusGraph`.
`uri_cli`|A `CURL` command line that illustrates how to send a gremlin query with token-based authentication.
`uri_cli_1`|An alternative `CURL` command line that connects to the database instance.
`maps`|unused
`uri`|The `https` URI that is used when connecting to the service, which includes the host name of server and port number to connect to. See [Connecting an external application](./connecting-external.html).
`uri_direct_1`|An alternative `https` URI for connecting to the service.
`misc`|A parent field containing the `session`, `websocket` and `gremlin_console_yaml` fields.
`misc.session`| An `https` URI that one uses to retrieve a 60-minute authentication token that can be used to interact with the service. See [Connecting an external application - Token Authentication](./connecting-external.html#token-authentication).
`misc.websocket`|The `wss` URIs that are used when connecting to the service using Websockets. See [Connecting an external application - Websockets](./connecting-external.html#websockets).
`misc.gremlin_console_yaml`|YAML information that is used to configure the Gremlin console to connect to the service.  See [Connecting an external application - Gremlin Console](./connecting-external.html#gremlin-console).
{: caption="Table 1. Compose for JanusGraph credentials" caption-side="top"}

## Using the credentials

Your application needs to be connected to your service to use the service credentials. The following steps show how you can do this, using Node.js as an example.

### Connecting the application to the service

If you already have an {{site.data.keyword.cloud_notm}} application you can connect it to a service from your {{site.data.keyword.cloud_notm}} account:

1. Log in to your {{site.data.keyword.cloud_notm}} account
2. From your dashboard, select the application you want to connect to your service.
3. In the connections panel of the application _Overview_ page, click **Connect new** or **Connect existing** to connect to a new or existing {{site.data.keyword.cloud_notm}} service.

  - If you clicked **Connect existing**, a list of available services is shown. Select the service you want to connect your app to, and click **Connect**.
  - If you clicked **Connect new**, follow the steps to provision a new service instance. When you select a service from the catalogue to provision, the _Connect to_ field will be automatically completed with the name of your application.

If you have an application that you haven't uploaded to {{site.data.keyword.cloud_notm}} you can bind the application to the service before uploading it to {{site.data.keyword.cloud_notm}}: 

1. Log in to your {{site.data.keyword.cloud_notm}} account
2. Download and install the [Cloud Foundry CLI](https://github.com/cloudfoundry/cli) tool
3. Connect to {{site.data.keyword.cloud_notm}} in the command line tool and follow the prompts to log in.

  ```
  $ cf api https://api.ng.bluemix.net
  $ cf login
  ```

4. Open your application's `manifest.yml` file.

  - Change the `host` value to something unique. The host you choose will determinate the subdomain of your application's URL:  `<host>.mybluemix.net`.
  - Change the `name` value. The value you choose will be the name of the app as it appears in your {{site.data.keyword.cloud_notm}} dashboard.

5. Update the `services` value in `manifest.yml` to match the name of your service. `manifest.yml` will now look similar to this:

  ```
  ---
  applications:
  - name:    compose-janusgraph-sample-service
    host:    compose-janusgraph-sample-service
    memory:  256M
    services:
      - compose-janusgraph-sample-app
  ```

### Accessing and using the credentials

Your application needs to parse the Cloud Foundry environvent variables. In Node.js you can do this with the [cfenv](https://www.npmjs.com/package/cfenv) package.

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

3. Push the app to {{site.data.keyword.cloud_notm}}. When you push the app it will automatically be bound to the service.

  ```
  $ cf push
  ```

For more information on using applications with {{site.data.keyword.cloud_notm}} services, see [Adding a service to your app](https://console.bluemix.net/docs/services/reqnsi.html#add_service).