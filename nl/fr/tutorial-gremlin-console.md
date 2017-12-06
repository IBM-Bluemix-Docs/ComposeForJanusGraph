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

# Création et traversée d'un graphique à l'aide de la console Gremlin

Cette rubrique vous aidera à démarrer avec votre première base de données {{site.data.keyword.composeForJanusGraph_full}}. Pour ce tutoriel, nous utiliserons la console Gremlin pour envoyer des commandes au serveur JanusGraph.

## 1. Installation de Gremlin

Pour installer la console Gremlin :

1. Vérifiez qu'une version récente de Java est installée.
2. Téléchargez la [console Gremlin version 3.2.3](https://archive.apache.org/dist/tinkerpop/3.2.3/apache-tinkerpop-gremlin-console-3.2.3-bin.zip).
3. Décompressez le fichier téléchargé dans un répertoire de travail.
4. A l'aide de la ligne de commande ou du terminal, accédez au répertoire racine de la console Gremlin et vérifiez le téléchargement en exécutant `bin/gremlin.sh` :

  ```text
  $ unzip -q  ~/Downloads/apache-tinkerpop-gremlin-console-3.2.3-bin.zip
  $ cd apache-tinkerpop-gremlin-console-3.2.3
  $ bin/gremlin.sh

          \,,,/
          (o o)
  -----oOOo-(3)-oOOo-----
  plugin activated: tinkerpop.server
  plugin activated: tinkerpop.utilities
  plugin activated: tinkerpop.tinkergraph
  gremlin> [CONTROL-D]                                                             $

  ```

5. Configurez la console Gremlin. Vous trouverez le fichier YAML de la console Gremlin sur la page *Vue d'ensemble* de votre service {{site.data.keyword.composeForJanusGraph}}. Sauvegardez l'une des configurations dans un fichier dans le répertoire `conf`. Dans cet exemple, `conf/compose.yaml`.
 
## 2. Connexion à Compose for JanusGraph

Pour vérifier la connexion, exécutez de nouveau `bin/gremlin.sh`. Ensuite, utilisez la commande `:remote` pour vous connecter au serveur tinkerpop :

```text
:remote connect tinkerpop.server conf/compose.yaml session
```

Cette commande demande à Gremlin de se connecter (`connect`) à un élément `tinkerpop.server` en utilisant la configuration que ocntient `conf/compose.yaml`. Nous transmettons également un autre argument, `session`, pour activer la prise en charge de la session.

Entrez `:help :remote` pour afficher les options de la commande `:remote`.
{: .tip}

Si la connexion aboutit, un avertissement SSL et un message s'affichent pour confirmer la connexion.

```text
WARN  org.apache.tinkerpop.gremlin.driver.Cluster  - SSL configured without a trustCertChainFile and thus trusts all certificates without verification (not suitable for production)
==>Configured portal93-4.ianuspater.compose-3.composedb.com/159.8.153.216:18045-[2378d0de-0a93-4330-b8d8-848667f5b117]
gremlin>
```

## 3. Envoi de commandes au serveur

La console Gremlin n'envoie pas automatiquement tous le éléments au serveur distant ; pour envoyer une commande au serveur, vous devez ajouter le préfixe `:>` à la commande. Vous pouvez également rediriger toutes les commandes de console vers le serveur distant avec `:remote console`. La série de commandes et réponses suivante illustre ce propos :

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

## 4. Création du graphique

{{site.data.keyword.composeForJanusGraph}} dispose d'une fabrique de graphiques dédiée pour la création, l'ouverture et la fermeture de graphiques. En utilisant cette fabrique (`ConfiguredGraphFactory`), vous n'avez pas besoin de connaître les mécanismes de stockage sous-jacents et la création d'un graphique consiste donc simplement à le nommer. Commencez par créer un graphique. Dans l'exemple de commandes de ce tutoriel, nous nommons notre graphique _mygraph_.

Utilisez le mot clé `def` pour déclarer la variable graph.

```
def graph=ConfiguredGraphFactory.create("mygraph")
```

Les noms de graphique {{site.data.keyword.composeForJanusGraph}} ne peuvent comporter que des caractères alphanumériques et des caractères de soulignement.
{: .tip}

## 5. Ouverture du graphique

Après avoir créé un graphique, vous devez l'ouvrir pour l'utiliser. Pour ce faire, vous appelez `open()` sur `ConfiguredGraphFactory`. Ouvrons notre graphique :

```
def graph=ConfiguredGraphFactory.open("mygraph")
```

A ce stade, nous devons valider l'opération pour conserver le graphique :

```
graph.tx().commit()
```

Si nous fermons la session maintenant, nous retrouverons notre graphique lorsque nous reviendrons.

## 6. Ajout de sommets au graphique

Notre graphique contiendra des informations relatives à des développeurs de logiciels ainsi que les éléments de logiciel qu'ils ont développés. Ajoutons une personne nommée "marko" et un élément de logiciel nommé "lop".

Commençons par ajouter marko.

```
def v1 = graph.addVertex(T.label, "person", "name", "marko", "age", 29)
```

Si la commande aboutit, elle renvoie l'index du sommet que nous avons ajouté, `==>v[4304]`, et nous pouvons poursuivre pour ajouter un second sommet.

```
def v2 = graph.addVertex(T.label, "software", "name", "lop", "lang", "java")
```

Une fois de plus, nous devons valider les opérations.

```
graph.tx().commit()
```

## 7. Recherche d'un sommet dans le graphique

Pour rechercher un sommet, disons celui nommé "marko", nous commençons par traverser le graphique, puis nous utilisons la méthode has() qui renvoie l'identificateur du sommet.

```
def g=graph.traversal()
g.V().has("name","marko")
```

Pour réaffecter nos sommets aux variables v1 et v2, par exemple si nous avons fermé notre ancienne session et en avons ouvert une nouvelle, nous utilisons next().

```
def v1 = g.V().has("name","marko").next()
def v2 = g.V().has("name","lop").next()
```

Nous pouvons ensuite utiliser ces variables dans les commandes Gremlin ultérieures.

## 8. Ajout d'une arête

Ensuite, nous pouvons ajouter entre les deux sommets une arête pondérée nommée "created" et de pondération 0,4.

Notez que vous devez convertir le chiffre en décimal en spécifiant la valeur sous la forme `0.4d`.

```
v1.addEdge("created", v2, "weight", 0.4d)
```

Une fois encore, veillez à valider la modification.

## 9. Interrogation du graphique

Vous pouvez maintenant interroger votre graphique pour trouver le nom de tous les logiciels que Marko a créé.

```
g.V().has("name","marko").out("created").values("name")
```
