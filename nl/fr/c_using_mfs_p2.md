---

copyright:
  years: 2016, 2018
lastupdated:  "2018-11-20"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen:  .screen}
{:codeblock:  .codeblock}
{:tip: .tip}
{:note: .note}

#	Configuration à l'aide du plan Professionnel, 1 application 
{: #using_mobilefoundation_p2}

Les utilisateurs du plan Professionnel, 1 application peuvent créer une application mobile avec différents systèmes d'exploitation mobiles.
Une fois que vous avez créé l'instance de service {{site.data.keyword.mobilefoundation_short}} : Professionnel, 1 application, lisez la procédure ci-après pour commencer à l'utiliser.

## Conditions prérequises
{: #prerequisites_p2}

Prenez connaissance des éléments suivants avant de configurer l'instance de service {{site.data.keyword.mobilefoundation_short}} : Professionnel, 1 application.
* {{site.data.keyword.mobilefoundation_short}} : Professionnel, 1 application est pris en charge uniquement avec les plans {{site.data.keyword.Db2_on_Cloud_short}} et {{site.data.keyword.composeForPostgreSQL}} {{site.data.keyword.Bluemix_notm}}.

* Vous devez avoir accès aux données d'identification de l'instance de service {{site.data.keyword.Db2_on_Cloud_short}} ou {{site.data.keyword.composeForPostgreSQL}} avant de pouvoir configurer les paramètres de votre instance de service {{site.data.keyword.mobilefoundation_short}}.

> **Remarque** : L'instance de service {{site.data.keyword.Db2_on_Cloud_short}} (tout plan autre que le plan **Lite**) ou {{site.data.keyword.composeForPostgreSQL}} peut exister dans n'importe quel `espace` de votre {{site.data.keyword.Bluemix_notm}} `organisation` ou de n'importe quelle autre `organisation` à laquelle vous avez accès. Assurez-vous que vous disposez des droits nécessaires pour accéder à l'`espace` dans lequel l'instance de service {{site.data.keyword.Db2_on_Cloud_short}} ou {{site.data.keyword.composeForPostgreSQL}} existe.


## Ajout de la connexion à la base de données
{: #configure_dashdb_p2}

###  Premières étapes
{: #firststeps_p2}

ne fois que vous avez créé l'instance de service {{site.data.keyword.mobilefoundation_short}} : Professionnel, 1 application, lisez la procédure ci-après pour commencer à l'utiliser.

### Configuration de la connexion à l'instance de service Db2 on Cloud
{: #connect_dashdb_p2}

Une fois l'instance de service {{site.data.keyword.mobilefoundation_short}} : Professionnel, 1 application créée, la page *Présentation* s'affiche. Vous devez spécifier ici les informations de connexion pour l'instance de service {{site.data.keyword.Db2_on_Cloud_short}} (tout plan autre que le plan **Lite**) ou {{site.data.keyword.composeForPostgreSQL}} à laquelle l'instance de service {{site.data.keyword.mobilefoundation_short}} doit se connecter.

Si vous ne possédez pas d'instance Db2 on Cloud existante, vous pouvez créer une instance de service {{site.data.keyword.Db2_on_Cloud_short}} (tout plan autre que le plan **Lite**) ou {{site.data.keyword.composeForPostgreSQL}}.

Suivez ces étapes pour créer une instance de service Db2 :

1. Sur la page *Présentation*, sélectionnez la section **Créer un service**.

+ Sélectionnez `Oui` pour l'option **Configuration à haute disponibilité** si vous souhaitez une instance de
service {{site.data.keyword.Db2_on_Cloud_short}} à haute disponibilité.

+ Examinez les détails du plan et cliquez sur **Créer**.

Une nouvelle instance de service {{site.data.keyword.Db2_on_Cloud_short}} est créée, qui fournit
une instance {{site.data.keyword.Db2_on_Cloud_short}} dédiée avec 8 Go de RAM, 2 vCPU (UC virtuelles) et 500 Go d'espace de stockage.

Suivez ces étapes pour vous connecter à une instance de service {{site.data.keyword.Db2_on_Cloud_short}} existante ou à
l'instance de service {{site.data.keyword.Db2_on_Cloud_short}} que vous venez de créer :

1. Sélectionnez l'`organisation` {{site.data.keyword.Bluemix_notm}} dans laquelle se trouve
l'instance de service {{site.data.keyword.Db2_on_Cloud_short}}.

+ Depuis la liste des espaces disponibles dans `Organisation`, sélectionnez l'`espace` {{site.data.keyword.Bluemix_notm}}
dans lequel se trouve l'instance de service {{site.data.keyword.Db2_on_Cloud_short}}.   
> **Remarque :** Si l'`organisation` et l'`espace` dans lesquels
réside votre instance de service {{site.data.keyword.Db2_on_Cloud_short}} ne figurent pas dans la liste, vérifiez que
vous êtes membre de cette `organisation` et de cet `espace`. Vous devez disposer d'un rôle *Développeur* pour accéder à l'organisation et à l'espace. Le service {{site.data.keyword.mobilefoundation_short}} accède aux données d'identification à partir du service {{site.data.keyword.Db2_on_Cloud_short}}.

+ Sélectionnez également le `Nom du service` et les
`Données d'identification` {{site.data.keyword.Db2_on_Cloud_short}}
pour la connexion à l'instance de service {{site.data.keyword.Db2_on_Cloud_short}} existante.

+  Testez la connexion à l'instance de service {{site.data.keyword.Db2_on_Cloud_short}} spécifiée.

+  Cliquez sur **Ajouter**. Cela permet de créer les tables requises dans l'instance de service de base de
données {{site.data.keyword.Db2_on_Cloud_short}} configurée.

Après quelques secondes, vous pouvez accéder à la page `Présentation`, qui fournit des tutoriels et des vidéos pour vous
permettre de faire vos premiers pas avec le service {{site.data.keyword.mobilefoundation_short}}.

Vous ne pouvez pas changer l'instance de service {{site.data.keyword.Db2_on_Cloud_short}} configurée pour être utilisée par votre instance de service {{site.data.keyword.mobilefoundation_short}}. Vous pouvez toutefois utiliser la même instance de service {{site.data.keyword.Db2_on_Cloud_short}} sur plusieurs instances de service {{site.data.keyword.mobilefoundation_short}}, car chaque instance de service {{site.data.keyword.mobilefoundation_short}} crée son propre schéma dans l'instance de service {{site.data.keyword.Db2_on_Cloud_short}} sélectionnée.
{: note}

## Démarrage du serveur MobileFirst
{: #start_mobilefoundation_p2}

* Pour démarrer le serveur
{{site.data.keyword.mfserver_short_notm}} avec les réglages par
défaut, cliquez sur **Démarrer le serveur de base**.

* Cette sélection crée une instance {{site.data.keyword.mfserver_long_notm}} avec les réglages suivants :
    -  1 Go de mémoire. Cette taille
suffit aux activités de développement et aux activités de test modérées, ainsi qu'aux charges
de travail de production à petite échelle.

    -	Le `nom d'utilisateur` et le `mot de passe` sont générés automatiquement pour vous. Vous pouvez y accéder une fois que le
serveur est en opération.

    Le processus de création de votre serveur commence. Ce processus prend environ 10 minutes et une fenêtre de
message indique la progression de l'opération. A son terme, un tableau de bord s'affiche et présente les éléments suivants :

      -	L'état du serveur que vous exécutez (état, taille).

      -	La route de serveur créée pour vous. Utilisez cette route dans votre application mobile pour vous connecter à {{site.data.keyword.mfserver_short_notm}}.

      -	Vos `nom d'utilisateur` et `mot de passe` personnels
pour accéder à la console {{site.data.keyword.mfp_oc_short_notm}}. Le `mot de passe` est masqué. Cliquez sur l'icône **Afficher le mot de passe** pour le visualiser.

*	Cliquez sur **Lancer la console** pour ouvrir la console {{site.data.keyword.mfp_oc_short_notm}}.

Elle vous permet de gérer vos applications, adaptateurs et appareils mobiles, ainsi que l'utilisation de votre serveur en tant que serveur dorsal mobile, l'envoi de notifications push, etc.

## Recréation du serveur MobileFirst
{: #recreate_mobilefoundation_p2}

*	Cliquez sur **Recréer** pour recréer le serveur.

* Cette action arrête votre serveur existant et supprime les données. Une
nouvelle instance de serveur est créée avec une version mise à jour, si elle est disponible. Cette action prend quelques minutes avant de s'achever.

Les
données de votre instance de serveur précédente, y compris les informations sur les applications et les adaptateurs,
sont conservées dans l'instance de service {{site.data.keyword.Db2_on_Cloud_short}} configurée. Elles sont utilisées pour recréer le serveur.
{: note}

##	Mise en place d'une configuration avancée
{: #using_mfs_advanced_p2}

L'option **Démarrer le serveur avec la configuration avancée**
de la page `Présentation` permet de créer le serveur avec des réglages
avancés ou personnalisés. Vous pouvez également mettre à jour les paramètres du serveur
pour personnaliser sa configuration en cliquant sur l'onglet **Configuration**. {{site.data.keyword.mobilefoundation_short}}
vous permet d'accéder à certains paramètres avancés.

*	Dans l'onglet **Topologie**, vous pouvez sélectionner la
taille du serveur, ainsi que le nombre d'instances dont vous avez besoin. Avec sa mémoire de 1 Go, le serveur par défaut
est suffisant pour le développement et des tests peu intensifs.
  - Sélectionnez la taille appropriée pour votre serveur compte tenu de
vos besoins.

  - La zone **Instances** affiche le nombre de noeuds créés.

## Mobile Analytics
{: #mobile_analytics}

Le service Mobile Analytics est inclut et préconfiguré avec l'instance du plan de service Développeur Mobile Foundation.

* Lancez la console Mobile Analytics à partir de {{site.data.keyword.mfp_oc_short_notm}}.

Pour plus d'informations sur Mobile Analytics, vous pouvez vous référer
à [MobileFirst Foundation Operational Analytics](https://cloud.ibm.com/docs/services/mobileanalytics/mobileanalytics_overview.html#about-mobile-analytics){: new_window}.

Pour plus de détails, consultez la documentation [{{site.data.keyword.mobilefoundation_long}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/bluemix/){: new_window}.
