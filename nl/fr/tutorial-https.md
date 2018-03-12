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

# Création et traversée d'un graphique à l'aide de HTTPS

{{site.data.keyword.composeForJanusGraph_full}} est livré avec un exemple de base de données de graphiques, 'The Graph of the Gods'. Ce tutoriel utilise cette exemple de base de données pour illustrer certains des concepts JanusGraph. Suivez les étapes pour créer, ouvrir et traverser le graphique en utilisant CURL pour envoyer des commandes Gremlin. Pour afficher une représentation visuelle du graphique, voir la documentation [Initiation](http://docs.janusgraph.org/latest/getting-started.html) de JanusGraph. 

## 1. Connexion à Compose for JanusGraph

Pour vous connecter à votre service {{site.data.keyword.composeForJanusGraph}} en utilisant CURL vous devez utiliser une chaîne de connexion. La chaîne de connexion contient le nom d'utilisateur, le mot de passe, le serveur et le numéro de port requis pour se connecter à votre service. Vous trouverez cette chaîne sur la page _Vue d'ensemble_ du tableau de bord de votre service.

Avant de commencer ce tutoriel, vérifiez que vous pouvez vous connecter à l'aide de la chaîne de connexion en transmettant une simple commande Gremlin :

```
curl -XPOST -d '{"gremlin" : "1+1" }' "<CONNECTION STRING>"
```

Si la requête aboutit, JanusGraph renvoie une réponse telle que :

```
{
  requestId":"d2fe6ac7-95d9-4c79-a254-123648cad54d",
  "status":{
    "message":"",
    "code":200,
    "attributes":{}
  },
  "result":{
    "data":[2],
    "meta":{}
  }
}
```

Vous pouvez maintenant travailler avec le "Graph of the Gods".

Ajoutez votre chaîne de connexion à la fin de chaque commande CURL de ce tutoriel.
{: .tip}

## 2. Création du graphique

{{site.data.keyword.composeForJanusGraph}} dispose d'une fabrique de graphiques dédiée pour la création, l'ouverture et la fermeture de graphiques. En utilisant cette fabrique (`ConfiguredGraphFactory`), vous n'avez pas besoin de connaître les mécanismes de stockage sous-jacents et la création d'un graphique consiste donc simplement à le nommer. Commencez par créer un graphique nommé _Example_.

Le bac à sable JanusGraph nécessite que vous déclariez toutes les variables que vous utilisez. Pour déclarer des variables, vous pouvez utiliser le mot clé `def`. Par exemple, pour déclarer la variable graph :

```
curl -XPOST -d '{"gremlin": "def graph=ConfiguredGraphFactory.create(\"example\");0;"}'
```

Les noms de graphique {{site.data.keyword.composeForJanusGraph}} ne peuvent comporter que des caractères alphanumériques et des caractères de soulignement.
{: .tip}

Notez que la commande curl se termine par `;0;`. C'est une solution de contournement, car l'API de requête HTTP émet une erreur (`{"message":"Cannot get namespace of root","Exception-Class":"java.lang.IllegalArgumentException"}`) même si l'opération s'est correctement effectuée. Ceci affecte uniquement le type de graphique lorsque le code tente de le renvoyer.

## 3. Ouverture du graphique

Après avoir créé un graphique, vous devez l'ouvrir pour l'utiliser. Pour ce faire, vous appelez `open()` sur `JanusGraphConfiguredFactory`. Ouvrons notre graphique 'example'.

```
curl -XPOST -d '{"gremlin": "def graph=ConfiguredGraphFactory.open(\"example\");0;"}'
```

## 4. Chargement du "Graph of the Gods"

Pour charger l'exemple de graphique, vous appelez la méthode `GraphOfTheGods.loadWithoutMixedIndex()`, vous la transmettez à votre graphique 'example' ouvert et le modèle est automatiquement créé.

```
curl -XPOST -d '{"gremlin": "def graph=ConfiguredGraphFactory.open(\"example\"); GraphOfTheGodsFactory.loadWithoutMixedIndex(graph,true);"}'
```

## 5. Préparation de la traversée du graphique

Maintenant que vous avez créé et ouvert un graphique, et que vous y avez chargé quelques données, vous pouvez explorer le graphique. Pour vous déplacer dans un graphique et l'analyser, vous avez besoin d'une source de traversée, obtenue en appelant la méthode `traversal()` sur l'objet graphique. Cela signifie que le préfixe complet de toute commande sera :

```
curl -XPOST -d '{"gremlin": "def graph=ConfiguredGraphFactory.open(\"example\");def g=graph.traversal();0;"}'
```

L'utilisation de `graph` pour le graphique et de `g` pour la source de traversée est classique. Si vous n'avez besoin que de la source de traversée, vous pouvez compresser en :

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal();0;"}'
```

## 6. Traversée du graphique pour trouver Saturn

Le graphique "Graph of the Gods" contient un index global des propriétés de nom. Ce type d'index constitue souvent la première étape pour atteindre un point particulier du graphique. Par exemple, pour trouver "saturn" dans le graphique :

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next()"}'
```

Cette commande renvoie le vertex ayant la propriété `name: saturn`.

```
{
  "requestId":"34d349a7-fa8e-4268-8b8d-a8adc0056bf3",
  "status":{
    "message":"",
    "code":200,
    "attributes":{}
  },
  "result":{
    "data":[
      {
        "id":4184,
        "label":"titan",
        "type":"vertex",
        "properties":{
          "name":[
            {
              "id":"16z-388-sl",
              "value":"saturn"
            }
          ],
          "age":[
            {
              "id":"1l7-388-35x",
              "value":10000
            }
          ]
        }
      }
    ],
  "meta":{}
  }
}
```

## 7. Traversée du graphique pour trouver les enfants de Saturn

Pour approfondir l'exemple, en définissant le vertex "saturn" dans une variable 'saturn', vous pouvez demander quels vertex stockent une arête sur laquelle pointe le libellé 'father'. La méthode `in("father")` sélectionne les arêtes entrantes et la méthode `values("name")` renvoie la valeur de la propriété de nom de ce vertex.

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); ;g.V(saturn).in(\"father\").values(\"name\")"}'
```

La requête renvoie les noms des enfants de Saturn dans le graphique, ici un seul : "jupiter".

```
{
  "requestId":"2ad6c01e-f724-4e0a-8895-4e8e368fb7ca",
  "status":{
    "message":"",
    "code":200,
    "attributes":{}
  },
  "result":{
    "data":[
      "jupiter"
    ],
    "meta":{}
  }
}
```

Si vous incluez l'opération `in("father")` une seconde fois, votre traversée vous mène aux petit-enfants du vertex Saturn.

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); ;g.V(saturn).in(\"father\").in(\"father\").values(\"name\")"}'
```

Cette requête vous permet de trouver le nom du petit-enfant de Saturn, à savoir 'hercules'.

## 8. Traversée du graphique - Emplacement

Dans GraphOfTheGods, certaines arêtes ont une propriété "place", composée d'une latitude et d'une longitude. Cette propriété permet de géolocaliser des événements. Les arêtes libellées "battled" utilise la propriété place pour indiquer où des batailles ont eu lieu. Pour trouver toutes les batailles qui ont eu lieu dans un rayon de 50 km d'Athènes (latitude : 37.97 et longitude : 23.72), recherchez dans les arêtes du graphique toutes les valeurs de propriété "place" comprises dans le rayon de ces coordonnées.

Vous pouvez sélectionner ces arêtes à l'aide de `has("place", geoWithin(Geoshape.circle(37.97, 23.72, 50)))`.

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); g.E().has(\"place\", geoWithin(Geoshape.circle(37.97, 23.72, 50)))"}'
```

La requête renvoie le libellé d'arête pour les vertex entrants et sortants ainsi que leurs identificateurs. Pour que la réponse sont plus lisible et pour trouver qui a participé à ces batailles, vous pouvez utiliser `as()` pour stocker les valeurs renvoyées sous les noms _god1_ et _god2_, puis appeler une méthode `select()` pour extraire ces valeurs en utilisant `by("name")` pour extraire les noms des deux dieux impliqués dans chaque bataille.

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); g.E().has(\"place\", geoWithin(Geoshape.circle(37.97, 23.72, 50))).as(\"source\").inV().as(\"god2\").select(\"source\").outV().as(\"god1\").select(\"god1\", \"god2\").by(\"name\")"}'
```

```
"result":{
  "data":[
    {
      "god1":"hercules",
      "god2":"nemean"
    },
    {
      "god1":"hercules",
      "god2":"hydra"
    }
  ],
  "meta":{}
}
```

## 9. Traversée du graphique - Vertex

Dans une précédente étape, vous avez utilisé deux éléments `in()` pour identifier que Hercules était un petit-enfant de Saturn. La Traversée peut également être exprimée en boucle, car Gremlin peut répéter (`repeat`) un predicat :

```
curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); def hercules=g.V(saturn).repeat(__.in(\"father\")).times(2).next()"}'
```

Vous pouvez effectuer des opérations avancées qui utilisent le vertex Hercules comme point de départ. Par exemple, vous pouvez :

- Traversez les arêtes libellées "father" et "mother" détermine le "type" de parents qu'avait Hercules :

  ```
  curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); def hercules=g.V(saturn).repeat(__.in(\"father\")).times(2).next(); g.V(hercules).out(\"father\", \"mother\").label();" }'
  ```

- Traversez les arêtes "battled" pour voir qui Hercules a combattu :

  ```
  curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); def hercules=g.V(saturn).repeat(__.in(\"father\")).times(2).next(); g.V(hercules).out(\"battled\").valueMap()" }' 
  ```

- Traversez les arêtes "battled" pour voir qui Hercules a combattu plus d'une fois :

  ```
  curl -XPOST -d '{"gremlin": "def g=ConfiguredGraphFactory.open(\"example\").traversal(); def saturn=g.V().has(\"name\", \"saturn\").next(); def hercules=g.V(saturn).repeat(__.in(\"father\")).times(2).next(); g.V(hercules).outE(\"battled\").has(\"time\", gt(1)).inV().values(\"name\")" }' 
  ```
  
