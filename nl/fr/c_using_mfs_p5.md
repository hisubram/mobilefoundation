---

copyright:
  years: 2016, 2019
lastupdated: "2019-06-06"

keywords: mobile foundation, mobile analytics, professional plan, configure database

subcollection:  mobilefoundation
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen:  .screen}
{:codeblock:  .codeblock}
{:tip: .tip}
{:note: .note}

#	Configuration à l'aide du plan Professionnel par appareil
{: #using_mobilefoundation_p5}

Avec le plan Professionnel par appareil, les utilisateurs peuvent générer, tester et exécuter des applications mobiles en production, quel que soit le nombre d'utilisateurs ou d'appareils mobiles. Les frais sont basés sur le nombre d'appareils client quotidiens. Ce plan prend en charge les déploiements étendus et la haute disponibilité.
Une fois que vous avez créé l'instance de service {{site.data.keyword.mobilefoundation_short}} : Professionnel par appareil, lisez la procédure ci-après pour commencer à l'utiliser.

## Prérequis dans le plan Professionnel par appareil
{: #prerequisites_p5}

Prenez connaissance des éléments suivants avant de configurer l'instance de service {{site.data.keyword.mobilefoundation_short}} : Professionnel par appareil.
* {{site.data.keyword.mobilefoundation_short}} : Professionnel par appareil est pris en charge uniquement avec les plans {{site.data.keyword.Db2_on_Cloud_short}} (tout plan autre que le plan **Lite**) ou {{site.data.keyword.composeForPostgreSQL}} {{site.data.keyword.Bluemix_notm}}.

* Vous devez avoir accès aux données d'identification de l'instance de service {{site.data.keyword.Db2_on_Cloud_short}} (tout plan autre que le plan **Lite**) ou {{site.data.keyword.composeForPostgreSQL}} avant de pouvoir configurer les paramètres de votre instance de service {{site.data.keyword.mobilefoundation_short}}.

L'instance de service {{site.data.keyword.Db2_on_Cloud_short}} (tout plan autre que le plan **Lite**) ou {{site.data.keyword.composeForPostgreSQL}} peut exister dans n'importe quel `espace` de votre {{site.data.keyword.Bluemix_notm}} `organisation` ou de n'importe quelle autre `organisation` à laquelle vous avez accès. Assurez-vous que vous disposez des droits nécessaires pour accéder à l'`espace` dans lequel l'instance de service {{site.data.keyword.Db2_on_Cloud_short}} ou {{site.data.keyword.composeForPostgreSQL}} existe.
{: note}

## Ajout de la connexion à la base de données
{: #configure_dashdb_p5}

###  Premières étapes
{: #firststeps_p5}

Une fois que vous avez créé l'instance de service {{site.data.keyword.mobilefoundation_short}} : Professionnel par appareil, lisez la procédure ci-après pour commencer à l'utiliser.

### Configuration de la connexion à la base de données
{: #connect_dashdb_p5}

Une fois l'instance de service {{site.data.keyword.mobilefoundation_short}} : Professionnel par appareil créée, la page *Présentation* s'affiche et vous devez y spécifier les informations de connexion à l'instance de service {{site.data.keyword.Db2_on_Cloud_short}} (tout plan autre que le plan **Lite**) ou {{site.data.keyword.composeForPostgreSQL}} à laquelle l'instance de service {{site.data.keyword.mobilefoundation_short}} doit se connecter.

Vous pouvez également créer une instance de service {{site.data.keyword.Db2_on_Cloud_short}} (tout plan autre que le plan **Lite**) ou {{site.data.keyword.composeForPostgreSQL}}, si vous n'en disposez pas.

Suivez ces étapes pour créer une nouvelle instance de service Db2 on Cloud :

1. Sur la page *Présentation*, sélectionnez la section **Créer un service**.

+ Sélectionnez `Oui` pour l'option **Configuration à haute disponibilité** si vous souhaitez une instance de service {{site.data.keyword.Db2_on_Cloud_short}} à haute disponibilité.

+ Examinez les détails du plan et cliquez sur **Créer**.

Une nouvelle instance de service {{site.data.keyword.Db2_on_Cloud_short}} est créée, qui fournit une instance {{site.data.keyword.Db2_on_Cloud_short}} dédiée avec 8 Go de RAM, 2 vCPU (UC virtuelles) et 500 Go d'espace de stockage.

Suivez ces étapes pour vous connecter à une instance de service {{site.data.keyword.Db2_on_Cloud_short}} existante ou à
l'instance de service {{site.data.keyword.Db2_on_Cloud_short}} que vous avez créée :

1. Sélectionnez l'`organisation` {{site.data.keyword.Bluemix_notm}} dans laquelle se trouve l'instance de service {{site.data.keyword.Db2_on_Cloud_short}}.

+ Depuis la liste des espaces disponibles dans `Organisation`, sélectionnez l'`espace` {{site.data.keyword.Bluemix_notm}} dans lequel se trouve l'instance de service {{site.data.keyword.Db2_on_Cloud_short}}.   
Si l'`organisation` et l'`espace` dans lesquels réside votre instance de service {{site.data.keyword.Db2_on_Cloud_short}} ne figurent pas dans la liste, vérifiez que
vous êtes membre de cette `organisation` et de cet `espace`. Vous devez être affecté au rôle *Développeur* dans l'organisation et l'espace vu que le service {{site.data.keyword.mobilefoundation_short}}accède aux données d'identification du service {{site.data.keyword.Db2_on_Cloud_short}}.
{: note}
+ Sélectionnez également le `Nom du service` et les `Données d'identification` {{site.data.keyword.Db2_on_Cloud_short}} pour la connexion à l'instance de service {{site.data.keyword.Db2_on_Cloud_short}} existante.

+  Testez la connexion à l'instance de service {{site.data.keyword.Db2_on_Cloud_short}} spécifiée.

+  Cliquez sur **Ajouter**. Cela permet de créer les tables requises dans l'instance de service de base de données {{site.data.keyword.Db2_on_Cloud_short}} configurée.

Après quelques secondes, vous pouvez accéder à la page `Présentation`, qui fournit des tutoriels et des vidéos pour vous permettre de faire vos premiers pas avec le service {{site.data.keyword.mobilefoundation_short}}.

Vous ne pouvez pas changer l'instance de service {{site.data.keyword.Db2_on_Cloud_short}} configurée pour être utilisée par votre instance de service {{site.data.keyword.mobilefoundation_short}}. Vous pouvez toutefois utiliser la même instance de service {{site.data.keyword.Db2_on_Cloud_short}} sur plusieurs instances de service {{site.data.keyword.mobilefoundation_short}}, car chaque instance de service {{site.data.keyword.mobilefoundation_short}} crée son propre schéma dans l'instance de service {{site.data.keyword.Db2_on_Cloud_short}} sélectionnée.
{: note}

## Démarrage du serveur MobileFirst créé à l'aide du plan Professionnel par appareil
{: #start_mobilefoundation_p5}

* Pour démarrer le serveur {{site.data.keyword.mfserver_short_notm}} avec les réglages par défaut, cliquez sur **Démarrer le serveur de base**.

* Cette sélection crée une instance {{site.data.keyword.mfserver_long_notm}} avec les réglages suivants :
    -  Deux noeuds avec 1 Go de mémoire chacun. Cette taille
convient aux activités de développement et aux activités de test modérées, ainsi qu'aux charges
de travail de production à petite échelle.

    -	Le `nom d'utilisateur` et le `mot de passe` sont générés automatiquement pour vous. Vous pouvez y accéder une fois que le serveur est en opération.

Le processus de création de votre serveur commence. Ce processus prend environ 10 minutes et une fenêtre de message indique la progression de l'opération. A son terme, un tableau de bord s'affiche et présente les éléments suivants :

  -	L'état du serveur que vous exécutez (état, taille).

  -	La route de serveur créée pour vous. Utilisez cette route dans votre application mobile pour vous connecter à {{site.data.keyword.mfserver_short_notm}}.

  -	Vos `nom d'utilisateur` et `mot de passe` personnels pour accéder à la console {{site.data.keyword.mfp_oc_short_notm}}. Le `mot de passe` est masqué. Cliquez sur l'icône **Afficher le mot de passe** pour le visualiser.

*	Cliquez sur **Lancer la console** pour ouvrir la console {{site.data.keyword.mfp_oc_short_notm}}.


Elle vous permet de gérer vos applications, adaptateurs et
appareils mobiles, ainsi que l'utilisation de votre serveur en
tant que serveur dorsal mobile, l'envoi de notifications push, etc.

## Re-création du serveur MobileFirst lors de l'utilisation du plan Professionnel par appareil
{: #recreate_mobilefoundation_p5}

*	Cliquez sur **Recréer** pour recréer le serveur.

* Cette action arrête votre serveur existant et supprime les données. Une nouvelle instance de serveur est créée avec une version mise à jour, si elle est disponible. Cette action prend quelques minutes avant de s'achever.

Les données de votre instance de serveur précédente, qui incluent des informations sur les applications et les adaptateurs,
sont conservées dans l'instance de service {{site.data.keyword.Db2_on_Cloud_short}} configurée. Elles sont utilisées pour recréer le serveur.
{: note}

##	Configuration avancée dans le plan Professionnel par appareil
{: #using_mfs_advanced_p5}

L'option **Démarrer le serveur avec la configuration avancée** de la page `Présentation` permet de créer le serveur avec des réglages avancés ou personnalisés. Vous pouvez également modifier les réglages du serveur pour personnaliser sa
configuration en cliquant sur l'onglet **Paramètres**. {{site.data.keyword.mobilefoundation_short}} vous permet d'accéder à certains paramètres avancés.

*	Dans l'onglet **Topologie**, vous pouvez sélectionner la taille du serveur, ainsi que le nombre d'instances dont vous avez besoin. Avec sa mémoire de 1 Go, le serveur par défaut est suffisant pour le développement et des tests peu intensifs.
  - Sélectionnez la taille appropriée pour votre serveur compte tenu de vos besoins.

  - La zone **Instances** affiche le nombre d'instances créées.

## Mobile Analytics dans le plan Professionnel par appareil
{: #mobile_analytics_p5}

Le service Mobile Analytics est inclut et préconfiguré avec l'instance du plan de service Développeur Mobile Foundation.

* Lancez la console Mobile Analytics à partir de {{site.data.keyword.mfp_oc_short_notm}}.

Pour plus d'informations sur Mobile Analytics, reportez-vous [ici](/docs/services/mobilefoundation?topic=mobilefoundation-instrument_your_app#instrument_your_app){: new_window}.
