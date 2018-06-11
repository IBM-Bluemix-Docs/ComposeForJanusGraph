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
{:tip: .tip}

# Paramètres

Ces fonctions vous permettent d'adapter votre service {{site.data.keyword.composeForJanusGraph_full}} de sorte qu'il réponde mieux à vos besoins et exigences.


## Mise à niveau de la version

Lorsqu'une nouvelle version de la base de données est disponible, un menu déroulant s'affiche. Il vous permet de sélectionner la version vers laquelle vous voulez effectuer une mise à niveau. Si votre service est à la version la plus récente disponible, le panneau affiche les informations relatives à cette version.

![Panneau Version](./images/janusgraph-version-show.png "Panneau Version")


## Mise à l'échelle des ressources

Si votre service a besoin de plus d'espace de stockage, ou si vous voulez limiter la quantité de stockage allouée au service, vous pouvez effectuer une mise à l'échelle des ressources.

1. Accédez à la page _Overview_ de votre service.
2. Dans le panneau _Deployment Details_, cliquez sur **Scale Resources**. La page Scale Resources s'ouvre.

    ![Page Scale Resources](./images/janusgraph-scale-show.png "Page Scale Resources")

3. Déplacez le curseur sur la règle pour augmenter ou diminuer la quantité de stockage allouée au service {{site.data.keyword.composeForJanusGraph}}. Déplacez-le vers la gauche pour diminuer la quantité de stockage ou vers la droite pour l'augmenter.
4. Cliquez sur **Scale Deployment** pour lancer le processus de mise à l'échelle et revenir dans la vue d'ensemble du tableau de bord. 

Une fois la mise à l'échelle terminée, le panneau _Deployment Details_ est actualisé pour afficher l'utilisation actuelle et la nouvelle valeur de stockage disponible.

### Mise à l'échelle de la mémoire

Si vous disposez d'un jeu de données relativement peu important mais que vous devez exécuter des requêtes complexes, vous pouvez conserver la taille par défaut de 5 Go pour les noeuds de stockage et mettre à l'échelle la mémoire des noeuds JanusGraph Engine afin d'améliorer les performances des requêtes. Pour ajouter de la mémoire aux noeuds JanusGraph Engine, contactez le support.


## Mise à jour du mot de passe

Il peut s'avérer nécessaire de modifier le mot de passe de votre service. La fonction _Mettre à jour le mot de passe_ permet d'effectuer cette modification. 

Un nouveau mot de passe aléatoire est généré ou vous pouvez entrer votre propre mot de passe dans la zone. Pour générer un autre mot de passe, cliquez sur le dé à jouer en regard de la zone. 
  
![Mise à jour du mot de passe etcd](./images/janusgraph-update-password.png "Générateur de mot de passe automatique")

Cliquez sur **Mise à jour du mot de passe**. Un message vous demande de confirmer la modification. Cliquez sur **Mise à jour du mot de passe** dans la boîte de dialogue pour confirmer le nouveau mot de passe ou sur Annuler pour annuler la modification. Le panneau _Deployment Details_ affiche la progression de l'exécution du travail.

**Remarque :** Cette opération modifie les données d'identification que vous utilisez ainsi que vos services pour vous connecter et invalide la chaîne de connexion de votre service. Elle peut également entraîner un temps d'indisponibilité.

### Mise à jour des applications connectées
La modification du mot de passe invalide la chaîne de connexion existante et en génère une nouvelle. Cela provoque une interruption du service jusqu'à ce que les applications connectées soient mises à jour avec la nouvelle chaîne de connexion. Vous devrez effectuer cette opération en fournissant la nouvelle chaîne de connexion à toutes vos applications.

Vous trouverez plus d'informations sur la connexion de vos applications dans [Connexion d'une {{site.data.keyword.cloud}} application](./connecting-bluemix-app.html)
et [Connexion d'une application externe](./connecting-external.html).


## Utilisation des listes blanches

Si vous voulez limiter l'accès à vos bases de données, vous pouvez constituer une liste blanche d'adresses IP ou de plages d'adresses IP sur votre service. Lorsque la liste blanche ne contient aucune adresse IP, elle est désactivée et le déploiement accepte les connexions de tout système sur internet.

![Adresses IP mises en liste blanche](./images/janusgraph-whitelist-show.png "Zones d'une liste blanche.")

### Adresses IP
La zone *IP* accepte une adresse IPv4 ou IPv6 complète, avec ou sans masque de réseau. Sans masque de réseau, les connexions entrantes doivent provenir très exactement de cette adresse IP. 

Même si la zone IP accepte les adresses IPv6, aucun déploiement Compose n'est actuellement disponible pour un réseau IPv6 de sorte que ces adresses ne peuvent pas être filtrées.

### Masques de réseau
Pour autoriser une connexion à partir d'une plage spécifiée d'adresses IP, utilisez un masque de réseau. L'adresse IP doit être intégralement spécifiée lorsque vous utilisez un masque de réseau. Cela signifie que vous devez, par exemple, entrer 192.168.1.0/24 plutôt que 192.168.1/24.

### Description
La *Description* peut être tout texte significatif pour l'utilisateur lui permettant d'identifier l'entrée de la liste blanche, par exemple, nom de client, identificateur de projet ou matricule. La zone Description est obligatoire.

### Services Compose
Les entrées d'une liste blanche sont automatiquement ajoutées aux serveurs de Compose pour leur permettre de se connecter.

### Suppression
Pour supprimer une adresse IP ou un masque de réseau de la liste blanche, cliquez sur l'entrée *Retirer* en regard de l'élément à supprimer.
Lorsque toutes les entrées d'une liste blanche ont été supprimées, la liste est désactivée et toutes les adresses IP sont acceptées par les portails d'accès TCP.
