---
copyright:
  years: '2015, 2017'
lastupdated: '2017-11-21'
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Notes sur JanusGraph hébergé

Etant donné que {{site.data.keyword.composeForJanusGraph_full}} est un service hébergé, garantir la sécurité des déploiements diffère quelque peu par rapport à JanusGraph s'exécutant sur un serveur dédié. Cette rubrique met en évidence ces différences et variations. La plupart de ces différences affectent la manière dont fonctionnent les requêtes du langage Gremlin.

## Bacs à sable

Pour garantir que les requêtes Gremlin n'accèdent pas à des fonctionnalités susceptibles de compromettre le système, ces requêtes sont toutes exécutées dans un bac à sable. Cela signifie que tous les appels de fonction etde  méthode doivent avoir une signature qui correspond à l'une des expressions régulières suivantes :

```
java\.util.*
java\.lang\.Object.*
java\.lang\.Class <T extends java\.lang\.Object>#getSimpleName\(
java\.lang\.Iterable <T extends java\.lang\.Object>#iterator\(\)
java\.lang\.Boolean(?!#getBoolean\().*
java\.lang\.Double.*
java\.lang\.Float.*
java\.lang\.Integer(?!#getInteger\().*
java\.lang\.Long(?!#getLong\().*
java\.lang\.Math.*
java\.lang\.String(?!#intern\(\)).*
java\.lang\.StringBuilder.*
java\.lang\.Object#equals\(
java\.lang\.Object#hashCode\(
java\.util\.ArrayList#equals\(

org\.codehaus\.groovy\.runtime\.DefaultGroovyMethods.*
org\.codehaus\.groovy\.runtime\.StringGroovyMethods.*
org\.apache\.commons\.configuration\.MapConfiguration.*
org\.apache\.tinkerpop\.gremlin\.structure(?!\.io|\.Graph#toString\(|\.Graph#variables\(|\.Graph#configuration\(|\.Graph#io\(\)|\.util\.GraphFactory.*).*
org\.apache\.tinkerpop\.gremlin\.process\.traversal.*
org\.apache\.tinkerpop\.gremlin\.util\.Gremlin#version\(\)
org\.janusgraph\.core(?!\.JanusGraphFactory.*).*
org\.janusgraph\.graphdb(?!\.tinkerpop\.JanusGraphBlueprintsGraph#toString\(|\.tinkerpop\.JanusGraphBlueprintsGraph#variables\(|\.tinkerpop\.JanusGraphBlueprintsGraph#configuration\(|\.tinkerpop\.JanusGraphBlueprintsGraph#io\().*
org\.janusgraph\.example\.GraphOfTheGodsFactory(?!#create\(|#main\().*
org\.apache\.tinkerpop\.gremlin\.groovy\.plugin\.dsl\.credential\.CredentialGraph.*

Script\d+#.+\(?.+\)
groovy\.lang\.Closure <V extends java\.lang\.Object>#call\(?.+\)

org\.janusgraph\.util\.stats\.MetricManager#getRegistry\(\)
org\.janusgraph\.util\.stats\.MetricManager#INSTANCE
org\.apache\.tinkerpop\.gremlin\.util\.MetricManager#getRegistry\(\)
org\.apache\.tinkerpop\.gremlin\.util\.MetricManager#INSTANCE
```

Toute tentative d'exécution d'un appel de fonction ou de méthode qui ne correspond à aucune des entrées de la liste blanche des fonctions entraîne une erreur : 

```
[Static type checking] - Not authorized to call this method: ...
```

Le bac à sable nécessite également que toutes les variables soit déclarées de manière statique afin d'activer le contrôle de la sécurité. Pour ce faire, vous disposez de la commande `def`.

## Sessions

Une session autorise une connexion à {{site.data.keyword.composeForJanusGraph}} afin de gérer l'état entre les requêtes. La plupart des options de connexion à {{site.data.keyword.composeForJanusGraph}} n'ont pas de sessions associées. Cela signifie que tout script Gremlin envoyé par le biais de ces méthodes de connexion doit être totalement autonome. Par exemple, à l'ouverture d'un graphique avec lequel le script a besoin de travailler, la requête des noeuds appropriés et la traversée du graphique à partir de ces noeuds doivent être gérées dans une seule requête. Cette restriction s'applique aux requêtes HTTP comme aux connexions WebSockets. 

La seule exception à cette règle est la connexion depuis la console Gremlin avec `:remote connect`.

```
:remote connect tinkerpop.server conf/compose.yaml
```

Si vous établissez une connexion avec la commande ci-dessus, il n'y a pas de session et la connexion opère de la même manière que des requêtes HTTP. Mais si vous ajoutez `session` comme argument, la prise en charge de session est alors activée.

```
:remote connect tinkerpop.server conf/compose.yaml session
```

En utilisant une session, vous pouvez définir une variable dans une commande et y faire référence dans une autre commande.
{: .tip}

## Transactions

Toutes les modifications apportées à la base de données JanusGraph sous-jacente sont encapsulées dans une transaction. Les transactions permettent que toutes les modifications qui leur sont apportées soient validées de manière à les rendre permanentes ou annulées pour garantir qu'elles ne le sont pas. 

Lorsque vous envoyez une demande via une requête HTTP ou via WebSocket, une transaction est automatiquement démarrée lorsque vous apportez une modification qui devrait s'inscrire dans la base de données. Cette transaction est également automatiquement validée une fois la requête traitée.

La seule exception est lorsque vous vous connectez à l'aide de la console Gremlin avec session activée. Il peut s'agir d'une session à long terme de sorte que c'est à l'utilisateur de valider toutes les modifications en appelant `graph.tx().commit()`, où `graph` est le graphique ouvert qui contient les modifications. Vous pouvez également appeler `graph.tx().rollback()` pour revenir à l'état antérieur du graphique avant le début de la transaction. 

## Index mixtes

La version actuelle de {{site.data.keyword.composeForJanusGraph}} ne prend pas en charge les index mixtes.
