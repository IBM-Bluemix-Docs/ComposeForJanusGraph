---

copyright:
  years: 2017,2018
lastupdated: "2017-12-12"
---

# Concepts JanusGraph

JanusGraph est une base de données de graphiques. Vous pouvez créer, interroger et modifier un grand nombre des graphiques de la base de données. JanusGraph est construit sur la pile Tinkerpop d'Apache et utilise le langage Gremlin pour les traversées, les commandes et les requêtes.

Pour plus d'informations sur JanusGraph, voir la [documentation JanusGraph](http://docs.janusgraph.org/latest/index.html).

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="//www.youtube.com/embed/zTaoMWv6lnE?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

D'autre vidéos concernant {{site.data.keyword.composeForJanusGraph}} sont disponibles dans le [centre d'apprentissage IBM Compose for JanusGraph](http://ibm.biz/janusgraph-learning).
{: .tip}

## Introduction aux bases de données de graphiques

Dans sa configuration la plus simple, un graphique est un ensemble de sommets avec des arêtes qui les relient. Le sommet est l'unité fondamentale du graphique et représente un objet indivisible. En termes clairs, cet objet peut être une personne, un emplacement ou un objet.  Il peut avoir des propriétés qui décrivent l'objet. 

Une arête est une connexion entre deux sommets qui symbolise une relation entre ces sommets. Elle peut avoir une multiplicité, une direction et des propriétés.

Dans les bases de données non graphiques, les relations entre des entités sont une fonction secondaire de la base de données, exprimées par le biais de zones de saisie partagées ou créées via des jointures. Cela signifie que le suivi des relations peut nécessiter que de nombreuses requêtes ou applications effectuent le travail d'assemblage des relations à partir de requêtes plus générales.

Dans les bases de données de graphiques, la relation est le premier composant du modèle de données et la traversée d'un sommet vers une arête vers un sommet et au-delà est le principal mécanisme d'interrogation des données dans le modèle. Les bases de documents de graphiques sont donc bien plus appropriées s'agissant de requêtes qui impliquent des relations telles que "toute les personnes qui aiment la marque X et qui ont un ami, ou un ami d'ami, qui achète la marque Y". 

Dans JanusGraph, chaque graphique a un nom unique. Pour créer, ouvrir, modifier et interroger un graphique, vous utilisez le langage Gremlin. Une requête Gremlin est composée de commandes capables de manipuler ou de traverser le graphique de manière à fournir le résultat escompté.

## Sommets

Un sommet est un élément de graphique qui représente un objet. A sa création, un identificateur défini par le moteur de graphique et un libellé défini par l'utilisateur sont attribués au sommet. Un sommet peut en outre stocker des propriétés qui peuvent être indexées et faire l'objet d'une requête.

Pour plus d'informations sur la création, l'interrogation et autres détails d'implémentation liés aux sommets, voir la [documentation Tinkerpop/Gremlin](http://tinkerpop.apache.org/docs/3.2.3/reference/#_the_graph_structure).

## Arêtes

Une arête est un élément de graphique qui représente une relation. Pour créer une arête, l'utilisateur fournit les sommets entrants/sortants et un libellé. Le moteur de graphique affecte un identificateur à l'arête. Une arête peut en outre stocker des propriétés qui peuvent être indexées et faire l'objet d'une requête.

Pour plus d'informations sur la création, l'interrogation et autres détails d'implémentation liés aux arêtes, voir la [documentation Tinkerpop/Gremlin](http://tinkerpop.apache.org/docs/3.2.3/reference/#_the_graph_structure).

## Propriétés

Une propriété est une paire clé:valeur qui décrit, nomme ou est associée à une arête ou un sommet. Les arêtes comme les sommets peuvent avoir plusieurs propriétés. Par exemple, si le sommet est de type 'humain', vous pouvez affecter à ce sommet les propriétés 'nom' et 'âge'.

Les propriétés peuvent être indexées de manière à pouvoir retrouver des sommets et des arêtes d'après leurs propriétés, par exemple, tous les sommets ayant le même nom.

Les propriétés des arêtes peuvent être des éléments tels que 'poids', de sorte qu'une traversée graphique de sommets via de multiples arêtes ayant des poids différents peut calculer le poids total de la traversée. 

## Traversée

Une traversée est le processus d'analyse de la structure d'un graphique. Ce processus découvre et renvoie des informations concernant des arêtes, des sommets et leurs propriétés. Pour plus d'informations sur les différentes étapes et les algorithmes d'une traversée, voir la [documentation sur les traversées de Tinkerpop/Gremlin](http://tinkerpop.apache.org/docs/3.2.3/reference/#traversal).

## Gremlin

Gremlin se divise en deux parties : le langage et le serveur.

Le langage Gremlin est un langage de traversée graphique utilisé pour interagir avec vos graphiques et les interroger.

Le serveur Gremlin est une spécification de serveur qui traite les requêtes en langage Gremlin locales ou distantes. Une implémentation de ce serveur fit partie d'Apache Tinkerpop.

Pour effectuer une requête en langage Gremlin, vous envoyez la requête Gremlin au serveur Gremlin approprié. Sur Compose, il s'agit d'un serveur qui s'exécute dans le cadre de votre déploiement JanusGraph.

## Apache TinkerPop

Apache TinkerPop est une infrastructure open source de traitement des graphiques. TinkerPop est le système qui modélise les données sous forme de graphique et qui interagit avec les commandes en langage Gremlin pour créer, traverser et manipuler des graphiques. Ppour plus d'informations sur la manière dont ces éléments s'articulent, voir la documentation TinkerPop/Gremlin sur l'[intégration du système graphique](http://tinkerpop.apache.org/docs/3.2.3/reference/#_graph_system_integration).
