---

copyright:
  years: 2017
lastupdated: "2017-09-13"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Connexion à JanusGraph

JanusGraph prend en charge les connexions en utilisant des requêtes HTTP via WebSockets. Toutes les connexions sont établies via HTTPS et nécessitent une authentification. Les requêtes basée sur HTTP prennent en charge l'authentification de base et par jeton. Pour toutes les connexions autres qu'un appel unique sur le déploiement JanusGraph, l'utilisation de WebSockets ou de l'authentification par jeton optimise les performances. L'authentification de base est un processus coûteux qui ralentit les requêtes.

Pour plus d'informations sur la manière d'utiliser des requêtes HTTP et pour apprendre à créer et ouvrir des graphiques, voir [Création et traversée d'un graphique à l'aide de HTTPS](./tutorial-https.html).
{: .tip}

La communication effective entre un client et une base de données implique que le client envoie des requêtes en Gremlin, dialecte du langage Groovy personnalisé pour l'analyse des structures graphiques, et que la base de données réponde à ces requêtes. Vous pouvez également envoyer des requêtes à l'aide de la console Gremlin.

Pour plus d'informations sur la manière d'obtenir, installer et configurer et utiliser la console Gremlin, voir [Création et traversée d'un graphique à l'aide de la console Gremlin](./tutorial-gremlin-console.html).
{: .tip}

## Requêtes HTTP

Si vous utilisez des requêtes HTTP pour vous connecter au serveur, il vous suffit pour communiquer avec JanusGraph d'un seul noeud final configuré pour fonctionner de façon circulaire dans tous les noeuds disponibles du déploiement. Ce noeud inclut le nom d'hôte et le port.

Vous pouvez obtenir vos chaînes de connexion dans la page Vue d'ensemble du tableau de bord de votre service. La chaîne de connexion nécessite une authentification de base à l'aide des données d'identification de l'administrateur pour se connecter au serveur.

Pour vous connecter à {{site.data.keyword.composeForJanusGraph}} avec une requête HTTP, vous devez utiliser la méthode HTTP POST avec le noeud final et envoyer un document formaté JSON avec une seule entrée contenant la clé "gremlin" et la valeur de la requête Gremlin que vous voulez envoyer. 

```json
{
    "gremlin": "a gremlin query would go here"
}
```

Gremlin est un langage de traversée développé, mais un programme rédigé dans ce langage peut se révélé aussi simple que "1+1". Pour transmettre cette valeur dans une commande `curl`, la partie données d'une requête POST est la suivante :

```
'{"gremlin" : "1+1" }'
``` 

Si vous envoyez plusieurs fois le même script au serveurs, vous améliorerez les performances en fournissant un dictionnaire de liaisons ("bindings") facultatif avec une mappe des variables disponibles pour le script gremlin. Les performances sont optimisées car le serveur n'a pas à recompiler le script gremlin.
{: .tip}

Vous trouverez des renseignements supplémentaires concernant le protocole HTTP dans la section [Connexion via REST](http://tinkerpop.apache.org/docs/3.2.3/reference/#_connecting_via_rest) de la Documentation Apache Tinkerpop. 

Pour envoyer un script Gremlin au noeud final, vous devez être authentifié. Il existe deux types d'authentification d'accès aux requêtes HTTP : l'authentification de base et l'authentification par jeton.

## Authentification de base

L'authentification de base envoie votre nom d'utilisateur et votre mot de passe avec la requête. Ce type d'authentification est envisageable pour des requêtes ponctuelles, mais sinon vous risquez d'encombrer le serveur avec les tâches complexes d'échange et de validation des mots de passe. Avec curl vous pouvez transmettre le nom d'utilisateur et le mot de passe à l'aide d'une option `-u` :

```shell
$ curl -XPOST -d '{"gremlin" : "1+1" }' "https://portal91-0.ianuspater.compose-3.composedb.com:17916" -u admin:PASSWORDHERE
```

Vous pouvez également intégrer le nom d'utilisateur et le mot de passe dans l'URL du noeud final. 

## Authentification par jeton

Pour que vos requêtes HTTP soient plus efficaces, vous pouvez obtenir un jeton de session d'un autre noeud final. Ce jeton est transmis dans l'en-tête de toutes les requêtes ultérieures. Si vous effectuez plus d'un appel ponctuel sur le noeud final HTTP, vous améliorerez les performances en utilisant l'authentification par jeton.

Utilisez l'une des chaînes de connexion du panneau _Session_ de la page Vue d'ensemble du tableau de bord du service. Les chaînes de connexion incluent déjà l'appel requis pour obtenir le jeton sous forme de commande curl :

```shell
$ curl -XGET "https://admin:PASSWORDHERE@portal91-0.ianuspater.compose-3.composedb.com:17916/session"
```

Le résultat JSON renvoyé contient le jeton de session :

```
{
  "token": "YWRtaW46MTQ5NjkyODM1NDgzMzpWYXlxMERsMWs0VmltdGo3WUpzMEwtN3prUi00TGxZR3J6LXZnbDVmN3lnPQ=="
}
```

Le jeton est valide pendant 60 minutes
{: .tip}

Extrayez la valeur de jeton et incluez-la en tant qu'en-tête `Authorization` dans toutes les requêtes ultérieures :

```shell
$ curl -XPOST -H "Authorization: Token YWRtaW46MTQ5NjkyODM1NDgzMzpWYXlxMERsMWs0VmltdGo3WUpzMEwtN3prUi00TGxZR3J6LXZnbDVmN3lnPQ==" -d '{"gremlin" : "1+1" }' "https://portal91-0.ianuspater.compose-3.composedb.com:17916"
```

Si vous effectuez ces opérations depuis le shell et que vous disposez de la commande [`jq`](https://stedolan.github.io/jq/), vous pouvez définir une variable d'environnement en exécutant une commande telle que :

```shell
$ export JGTOKEN=`curl -s -XGET "https://admin:PASSWORDHERE@portal91-0.ianuspater.compose-3.composedb.com:17916/session" | jq -r .token`
```

Exécutez ensuite des requêtes comme suit :

```shell
$ curl -XPOST -H "Authorization: Token $JGTOKEN" -d '{"gremlin" : "1+1" }' "https://portal91-0.ianuspater.compose-3.composedb.com:17916"
```

## Réponses JSON HTTP

Les réponses aux requêtes se présentent sous la forme d'un document JSON avec une valeur d'ID de requête, un objet de statut et un objet de résultat. Exemple de réponse :

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
`requestId` est l'identificateur unique de la requête. `status` contient un message en clair, un code de style de statut HTTPet d'autres attributs relatifs au statut indiqué. `result` contient un objet de données structuré selon ce que le code Gremlin exécuté renvoie et une section de métadonnées destinée à toutes les informations supplémentaires situées en dehors de la portée des données de résultat.
## Websockets

Les WebSockets sont un moyen efficace pour créer une connexion bidirectionnelle à long terme via TCP et TLS idéale pour des sessions interactives entre des applications client et le serveur JanusGraph. Un grand nombre des bibliothèques pour JanusGraph utilisent des WebSockets comme connexion au serveur ; pour utiliser JanusGraph sur Compose, elles doivent pouvoir effectuer une authentification de base et utiliser WSS, WebSockets sécurisés via TLS. 

Vous pouvez obtenir vos chaînes de connexion Websockets depuis la section _Websocket_ de la page Vue d'ensemble du tableau de bord du service. Des URI peuvent être utilisés pour établir une session à exécution longue avec le déploiement JanusGraph ; ils sont préfixés `wss:` pour indiquer que les connexions sont sécurisées par HTTPS. Ces chaînes de connexion nécessitent une authentification de base à l'aide des données d'identification de l'administrateur pour se connecter au serveur.

## Console Gremlin

La console Gremlin est l'outil essentiel pour utiliser des serveurs activés pour Tinkerpop. La console Gremlin utilise la connectivité des WebSockets, mais n'utilise pas la syntaxe URI pour les connexions et ce, en partie parce qu'elle doit également configurer d'autres éléments pour utiliser la connexion.

Pour configurer la console Gremlin vous fournissez un fichier YAML contenant les informations appropriées. Vous trouverez ces éléments de configuration dans le panneau _Gremlin Console YAML_ sur la page Vue d'ensemble du tableau de bord du service.

Pour plus d'informations sur la manière d'obtenir, installer et configurer et utiliser la console Gremlin, voir [Création et traversée d'un graphique à l'aide de la console Gremlin](./tutorial-gremlin-console.html).

