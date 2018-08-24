---

copyright:
  years: 2017,2018
lastupdated: "2018-05-08"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Connexion d'une application {{site.data.keyword.cloud_notm}}

Pour connecter une application {{site.data.keyword.cloud}} à votre service, utilisez les données d'identification créées lors de la mise à disposition du service.

## Données d'identification disponibles

Nom de zone|Description
----------|-----------
`deployment_id`|Identificateur interne du service, créé dans Compose.
`name`|Nom du déploiement de base de données.
`db_type`|Type de base de données fourni par le service, en l'occurrence, `JanusGraph`.
`uri_cli`|Ligne de commande `CURL` qui illustre comment envoyer une requête gremlin avec une authentification basée sur un jeton.
`uri_cli_1`|Seconde ligne de commande `CURL` qui permet d'établir la connexion à l'instance de base de données.
`maps`|non utilisé
`uri`|Identificateur URI `https` qui est utilisé pour la connexion au service et qui comprend le nom d'hôte du serveur et le numéro de port auquel se connecter. Voir [Connexion d'une application externe](./connecting-external.html).
`uri_direct_1`|Second identificateur URI `https` pour la connexion au service.
`misc`|Zone parent qui contient les zones `session`, `websocket` et `gremlin_console_yaml`.
`misc.session`| Identificateur URI `https` permettant d'extraire un jeton d'authentification valable 60 minutes pouvant être utilisé pour interagir avec le service. Voir [Connexion d'une application externe - Authentification par jeton](./connecting-external.html#token-authentication).
`misc.websocket`|Identificateurs URI `wss` utilisés lors de la connexion au service à l'aide de Websockets. Voir [Connexion d'une application externe - Websockets](./connecting-external.html#websockets).
`misc.gremlin_console_yaml`|Informations YAML utilisées pour configurer la console Gremlin en vue de la connexion au service.  Voir [Connexion d'une application externe - Console Gremlin](./connecting-external.html#gremlin-console).
{: caption="Tableau 1. Données d'identification Compose for JanusGraph" caption-side="top"}

## Utilisation des données d'identification

Votre application doit être connectée à votre service pour utiliser les données d'identification du service. Les étapes suivantes illustrent comment vous pouvez effectuer cette opération, en utilisant Node.js comme exemple.

### Connexion de l'application au service

Si vous disposez déjà d'une application {{site.data.keyword.cloud_notm}} vous pouvez la connecter au service à partir de votre compte {{site.data.keyword.cloud_notm}} :

1. Connectez-vous à votre compte {{site.data.keyword.cloud_notm}}.
2. A partir de votre tableau de bord, sélectionnez l'application que vous voulez connecter à votre service.
3. Dans le panneau Connexions de la page _Vue d'ensemble_ de l'application, cliquez sur **Connecter un nouveau** ou **Connecter un existant** pour établir une connexion à un service {{site.data.keyword.cloud_notm}} nouveau ou existant.

  - Si vous avez cliqué sur **Connecter un existant**, la liste des services disponibles s'affiche. Sélectionnez le service auquel vous voulez connecter votre application, puis cliquez sur **Connecter**.
  - Si vous avez cliqué sur **Connecter un nouveau**, suivez la procédure de mise à disposition d'une nouvelle instance de service. Lorsque vous sélectionnez dans le catalogue un service à mettre à disposition, la zone _Connecter à_ est automatiquement renseignée avec le nom de votre application.

Si vous avez une application que vous n'avez pas téléchargée dans {{site.data.keyword.cloud_notm}}, vous pouvez la lier au service avant de la télécharger dans {{site.data.keyword.cloud_notm}}: 

1. Connectez-vous à votre compte {{site.data.keyword.cloud_notm}}.
2. Téléchargez et installez l'outil [Interface de ligne de commande Cloud Foundry](https://github.com/cloudfoundry/cli)
3. Connectez-vous à {{site.data.keyword.cloud_notm}} dans l'outil de ligne de commande ete suivez les invites de connexion.

  ```
  $ cf api https://api.ng.bluemix.net
  $ cf login
  ```

4. Ouvrez le fichier `manifest.yml` de votre application.

  - Remplacez la valeur `host` par une valeur unique. L'hôte que vous choisissez détermine le sous-domaine de l'URL de l'application :  `<host>.mybluemix.net`.
  - Modifiez la valeur de `name`. La valeur que vous choisissez sera le nom de l'application tel qu'il apparaît dans votre tableau de bord {{site.data.keyword.cloud_notm}}.

5. Mettez à jour la valeur `services` dans `manifest.yml` de sorte qu'elle corresponde au nom de votre service. `manifest.yml` ressemblera à peu près à ceci :

  ```
  ---
  applications:
  - name:    compose-janusgraph-sample-service
    host:    compose-janusgraph-sample-service
    memory:  256M
    services:
      - compose-janusgraph-sample-app
  ```

### Accès et utilisation des données d'identification

Votre application a besoin d'analyser les variables d'environnement Cloud Foundry. Dans Node.js, vous pouvez effectuer cette opération à l'aide du package [cfenv](https://www.npmjs.com/package/cfenv).

1. Installez le package avec `--save` pour mettre à jour `package.json` :

  ```
  npm install --save cfenv
  ```

2. Extrayez les données d'identification {{site.data.keyword.composeForJanusGraph_full}} des variables d'environnement Cloud Foundry.

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

  Vous pouvez maintenant utiliser les données d'identification du service. Par exemple, pour vous procurer un identificateur URI de session permettant d'obtenir un jeton d'authentification :

  ```javascript
  let jgurl = credentials.misc.session[0];
  ```

3. Envoyez l'application à {{site.data.keyword.cloud_notm}}. Lorsque vous envoyez l'application, elle est automatiquement liée au service.

  ```
  $ cf push
  ```

Pour plus d'informations sur l'utilisation des applications avec des services {{site.data.keyword.cloud_notm}}, voir [Ajout d'un service à votre application](https://console.{DomainName}/docs/services/reqnsi.html#add_service).
