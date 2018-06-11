---

copyright:
  years: 2016,2018
lastupdated: "2018-03-27"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Tarification
{: #pricing}

## Configuration de base
Un service {{site.data.keyword.composeForJanusGraph_full}} démarre en tant que cluster de deux noeuds JanusGraph Engine, dotés chacun de 512 Mo de mémoire, ce qui équivaut à 2 unités de ressources. JanusGraph Storage est un cluster de trois noeuds, dotés chacun de 5 Go de stockage, ce qui équivaut à 5 unités de ressources. Le service _inclut_ la réplication et la haute disponibilité, de sorte que chaque unité JanusGraph Engine et le prix unitaire _incluent_ le coût des ressources dans les deux noeuds JanusGraph Engine. De même, chaque unité JanusGraph Storage et le prix unitaire incluent le coût des ressources dans les trois noeuds Storage.

La configuration de base inclut également deux portails HAProxy pour les accès, HTTPS et les listes blanches d'adresses IP. Ces portails disposent chacun de 64 Mo de mémoire.

### Coût
Le prix de la configuration du service de base est défini. Consultez les vignettes du catalogue sur {{site.data.keyword.cloud_notm}} pour connaître la tarification de base dans votre devise locale. Par exemple, le prix de base en dollars US est de 116 $/mois. Il inclut les 5 unités du cluster JanusGraph Storage à 90 $/mois et les 2 unités de JanusGraph Engine à 26 $/mois.


## Options d'extension
La configuration optimale de mémoire et de stockage variera selon l'utilisation et la charge de travail. Si vous avez besoin de davantage de stockage pour des jeux de données volumineux ou de plus de mémoire pour votre des requêtes complexes, vous pouvez augmenter les ressources allouées au stockage fourni par le cluster JanusGraph Storage et à la mémoire fournie par les noeuds JanusGraph Engine. 

Les clusters de stockage augmentent par unités de 1 Go et le prix unitaire _inclut_ le coût d'augmentation des ressources dans tous les noeuds du cluster. Une mise à l'échelle du stockage est disponible dans l'onglet _Paramètres_ du service.
 
Les noeuds JanusGraph Engine augmentent par unités de 256 Mo de mémoire et le prix unitaire _inclut_ le coût d'augmentation des ressources dans les deux noeuds. La mise à l'échelle de la mémoire n'est actuellement disponible qu'en prenant contact avec le support.

### Coût
Chaque unité supplémentaire JanusGraph Storage (1 Go) et chaque unité supplémentaire de mémoire JanusGraph Engine (256 Mo) a un prix unitaire indiqué dans votre devise locale dans la vignette du catalogue {{site.data.keyword.cloud_notm}} du service. En dollars US, chaque unité JanusGraph Storage coûte 18 $ et chaque unité JanusGraph Engine 13 $. Le prix unitaire diminue proportionnellement à l'augmentation de la taille _totale_ de vos services {{site.data.keyword.composeForJanusGraph}}, selon le barème indiqué dans le tableau de [tarification différenciée](#tiered-pricing).

## Tarification différenciée

### Tarification différenciée pour {{site.data.keyword.composeForJanusGraph}} Storage

Nombre d'unités JanusGraph Storage|Prix unitaire
----------|-----------
5 - 9 unités|Prix unitaire de base -- soit 18,00 USD/unité
10 - 24 unités|10 % de réduction -- soit 16,20 USD/unité
25 - 49 unités|20 % de réduction -- soit 14,40 USD/unité
50 - 99 unités|30 % de réduction -- soit 12,60 USD/unité
100 - 499 unités|40 % de réduction -- soit 10,80 USD/unité
500 - 999 unités|50 % de réduction -- soit 9,00 USD/unité
1000 - 4999 unités|60 % de réduction -- soit 7,20 USD/unité
5000 unités et plus|70 % de réduction -- soit 5,40 USD/unité
{: caption="Tableau 1. Tarification différenciée pour {{site.data.keyword.composeForJanusGraph}} Storage" caption-side="top"}

### Tarification différenciée pour {{site.data.keyword.composeForJanusGraph}} Engine

Nombre d'unités JanusGraph Engine|Prix unitaire
----------|-----------
2 - 9 unités|Prix unitaire de base -- soit 13,00 USD/unité
10 - 24 unités|10 % de réduction -- soit 11,70 USD/unité
25 - 49 unités|20 % de réduction -- soit 10,40 USD/unité
50 - 99 unités|30 % de réduction -- soit 9,10 USD/unité
100 - 499 unités|40 % de réduction -- soit 7,80 USD/unité
500 - 999 unités|50 % de réduction -- soit 6,50 USD/unité
1000 - 4999 unités|60 % de réduction -- soit 5,20 USD/unité
5000 unités et plus|70 % de réduction -- soit 3,90 USD/unité
{: caption="Tableau 2. Tarification différenciée pour {{site.data.keyword.composeForJanusGraph}} Engine" caption-side="top"}
