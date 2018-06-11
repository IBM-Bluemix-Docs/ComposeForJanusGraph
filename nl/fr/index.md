---

Copyright:
  years: 2017,2018
lastupdated: "2018-03-27"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Initiation à Compose for JanusGraph
{: #getting-started-with-compose-for-janusgraph}

{{site.data.keyword.composeForJanusGraph_full}} est une base de données de graphiques évolutive optimisée pour le stockage et l'interrogation de données hautement interconnectées, modélisée en millions ou milliards de sommets et arêtes. L'extraction simple efficace des données à partir de ces structures complexes est mise en oeuvre par la compatibilité Apache Tinkerpop(TM) de JanusGraph, qui permet d'effectuer des requêtes efficaces, difficiles, voire impossibles, avec une base de données relationnelle classique. {{site.data.keyword.composeForJanusGraph}} étend les fonctionnalités de JanusGraph en les gérant à votre place, vous offrant un système de déploiement évolutif facile à utiliser, qui fournit des fonctions de haute disponibilité et de redondance, des sauvegardes sans interruption automatisées, etc.
{:shortdesc}

Pour une présentation de {{site.data.keyword.composeForJanusGraph}}, voir [Concepts JanusGraph](./janusgraph-concepts.html)

## Création d'une instance de service Compose for JanusGraph

[Créez une instance {{site.data.keyword.composeForJanusGraph}}](https://console.bluemix.net/catalog/services/compose-for-janusgraph/).

Lorsque vous créez une instance du service, prenez soin de sélectionner un nom pour votre service et un nom de données d'identification. Laissez le service non lié ; vous pourrez connecter une application à votre service ultérieurement en utilisant les données d'identification fournies lors de la mise à disposition du service. Pour plus d'informations sur les diverses valeurs de données d'identification et la manière de les utiliser dans une application, voir [Connexion d'une application {{site.data.keyword.cloud}}](./connecting-bluemix-app.html).

Lorsque vous mettez à disposition l'instance de service, vous avez le choix entre le plan *Standard* et le plan *Entreprise*. Avec le plan *Entreprise*, vous pouvez mettre votre instance à disposition dans un cluster {{site.data.keyword.composeEnterprise}} disponible. {{site.data.keyword.composeEnterprise}} offre la sécurité et l'isolement requis par les normes de conformité des entreprises et utilise un réseau dédié pour garantir les performances des bases de données déployées. Pour plus d'informations, voir la [documentation Compose - Entreprise](../ComposeEnterprise/index.html).

## Gestion de Compose for JanusGraph

Vous pouvez gérer votre service depuis son tableau de bord. Vous y trouverez des informations concernant la base de données Compose d'{{site.data.keyword.cloud_notm}} et la manière de vous y connecter. Vous pouvez également :
- gérer vos sauvegardes ;
- allouer plus de ressources à votre service ;
- modifier le mot de passe du service ;
- constituer des listes blanches pour limiter l'accès à vos bases de données. 

Pour plus d'informations, voir [Paramètres](./dashboard-settings.html).

{{site.data.keyword.composeForJanusGraph}} repose sur les rôles Cloud Foundry pour la gestion de l'accès au service. Seuls les utilisateurs dotés du rôle Développeur peuvent voir ou utiliser le tableau de bord du service. Pour plus d'informations sur les rôles Cloud Foundry, voir les pages [Accès Cloud Foundry](https://console.bluemix.net/docs/iam/cfaccess.html#cfaccess) et [Gestion de l'accès Cloud Foundry](https://console.bluemix.net/docs/iam/mngcf.html#mngcf).
{: .tip}

## Connexion à Compose for JanusGraph

Vous pouvez vous connecter à votre service avec les données d'identification créées en même temps que le service ou avec les chaînes de connexion et la ligne de commande fournies dans l'onglet *Vue d'ensemble* du tableau de bord de votre service.

Pour vous connecter au service depuis l'extérieur d'{{site.data.keyword.cloud_notm}}, vous pouvez utiliser les chaînes de connexion ou la ligne de commande fournies. Pour plus d'informations, voir [Connexion à JanusGraph](./connecting-external.html).

## Connexion d'une application {{site.data.keyword.cloud_notm}}

Pour connecter une application {{site.data.keyword.cloud_notm}} à votre service, utilisez les données d'identification créées en même temps que le service. Pour plus d'informations sur la connexion d'une application {{site.data.keyword.cloud_notm}} à un service {{site.data.keyword.composeForJanusGraph}}, voir [Connexion d'une application {{site.data.keyword.cloud_notm}}](./connecting-bluemix-app.html).

## Utilisation du navigateur de données de JanusGraph

Explorer votre graphique de données à partir de la ligne de commande peut se révéler une tâche complexe. Le navigateur de données de {{site.data.keyword.composeForJanusGraph}} combine un générateur de requête facile à utiliser et des carte de réponse de requête qui affichent les résultats de vos requêtes sous forme de vue JSON interactive et de visualisation graphique. Pour plus d'informations, voir [Utilisation du navigateur de données JanusGraph](./data-browser.html)
