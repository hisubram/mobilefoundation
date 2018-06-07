---

copyright:
  years: 2016, 2018
lastupdated:  "2018-03-22"

---

#	Utilisation du plan Professionnel par appareil
{: #using_mobilefoundation_p5}

Avec le plan Professionnel par appareil, les utilisateurs peuvent générer, tester et exécuter des applications mobiles en production, quel que soit le nombre d'utilisateurs ou d'appareils mobiles, les frais dépendant du nombre d'appareils client utilisés quotidiennement. Ce plan prend en charge les déploiements étendus et la haute disponibilité.
Une fois que vous avez créé l'instance de service {{site.data.keyword.mobilefoundation_short}} : Professionnel par appareil, lisez la procédure ci-après pour commencer à l'utiliser.

## Conditions prérequises
{: #prerequisites_p5}

Prenez connaissance des éléments suivants avant de configurer l'instance de service {{site.data.keyword.mobilefoundation_short}} : Professionnel par appareil.
* {{site.data.keyword.mobilefoundation_short}} : Professionnel par appareil est pris en charge uniquement avec les plans {{site.data.keyword.Bluemix_notm}} {{site.data.keyword.Db2_on_Cloud_short}}.

* Vous devez avoir accès aux données d'identification de l'instance de service {{site.data.keyword.Db2_on_Cloud_short}} avant de pouvoir configurer les paramètres de votre instance de service {{site.data.keyword.mobilefoundation_short}}.

> **Remarque** : L'instance de service {{site.data.keyword.Db2_on_Cloud_short}} peut exister dans n'importe quel `espace` de votre {{site.data.keyword.Bluemix_notm}} `organisation` ou de n'importe quelle autre `organisation` à laquelle vous avez accès. Assurez-vous que vous disposez des droits nécessaires pour accéder à l'`espace` dans lequel l'instance de service {{site.data.keyword.Db2_on_Cloud_short}} service existe.


## Ajout de la connexion à la base de données
{: #configure_dashdb_p5}

###  Premières étapes
{: #firststeps_p5}

Une fois que vous avez créé l'instance de service {{site.data.keyword.mobilefoundation_short}} : Professionnel par appareil, lisez la procédure ci-après pour commencer à l'utiliser.

### Configuration de la connexion à l'instance de service Db2 on Cloud
{: #connect_dashdb_p5}

Une fois l'instance de service {{site.data.keyword.mobilefoundation_short}} : Professionnel par appareil créée, la page *Présentation* s'affiche et vous devez y spécifier les informations de connexion à l'instance de service {{site.data.keyword.Db2_on_Cloud_short}} à laquelle l'instance de service {{site.data.keyword.mobilefoundation_short}} doit se connecter.

Vous pouvez également créer une nouvelle instance de service {{site.data.keyword.Db2_on_Cloud_short}} si vous n'en avez pas encore.

Suivez ces étapes pour créer une nouvelle instance de service Db2 on Cloud :

1. Sur la page *Présentation*, sélectionnez la section **Créer un service**.

+ Sélectionnez `Oui` pour l'option **Configuration à haute disponibilité** si vous souhaitez une instance de
service {{site.data.keyword.Db2_on_Cloud_short}} à haute disponibilité.

+ Examinez les détails du plan et cliquez sur **Créer**.

Une nouvelle instance de service {{site.data.keyword.Db2_on_Cloud_short}} est créée, qui fournit
une instance {{site.data.keyword.Db2_on_Cloud_short}} dédiée avec 8 Go de RAM, 2 vCPU (UC virtuelles) et 500 Go d'espace de stockage.

Suivez ces étapes pour vous connecter à une instance de service {{site.data.keyword.Db2_on_Cloud_short}} existante ou à
l'instance de service {{site.data.keyword.Db2_on_Cloud_short}} que vous venez de créer :

1. Sélectionnez l'`organisation` {{site.data.keyword.Bluemix_notm}} dans laquelle se trouve l'instance de service {{site.data.keyword.Db2_on_Cloud_short}}.

+ Depuis la liste des espaces disponibles dans `Organisation`, sélectionnez l'`espace` {{site.data.keyword.Bluemix_notm}} dans lequel se trouve l'instance de service {{site.data.keyword.Db2_on_Cloud_short}}.   
> **Remarque :** Si l'`organisation` et l'`espace` dans lesquels réside votre instance de service {{site.data.keyword.Db2_on_Cloud_short}} ne figurent pas dans la liste, vérifiez que vous êtes membre de cette `organisation` et de cet `espace`. Vous
devez être affecté au rôle *Développeur* dans l'organisation et l'espace vu que le service {{site.data.keyword.mobilefoundation_short}}
accède aux données d'identification du service {{site.data.keyword.Db2_on_Cloud_short}}.

+ Sélectionnez également le `Nom du service` et les `Données d'identification` {{site.data.keyword.Db2_on_Cloud_short}} pour la connexion à l'instance de service {{site.data.keyword.Db2_on_Cloud_short}} existante.

+  Testez la connexion à l'instance de service {{site.data.keyword.Db2_on_Cloud_short}} spécifiée.

+  Cliquez sur **Ajouter**. Cela permet de créer les tables requises dans l'instance de service de base de
données {{site.data.keyword.Db2_on_Cloud_short}} configurée.

Après quelques secondes, vous pouvez accéder à la page `Présentation`, qui fournit des tutoriels et des vidéos pour vous
permettre de faire vos premiers pas avec le service {{site.data.keyword.mobilefoundation_short}}.

> **Remarque** : Vous ne pouvez pas changer l'instance de service {{site.data.keyword.Db2_on_Cloud_short}} configurée pour être utilisée par votre instance de service {{site.data.keyword.mobilefoundation_short}}. Vous pouvez toutefois utiliser la même instance de service {{site.data.keyword.Db2_on_Cloud_short}} sur plusieurs instances de service {{site.data.keyword.mobilefoundation_short}}, car chaque instance de service {{site.data.keyword.mobilefoundation_short}} crée son propre schéma dans l'instance de service {{site.data.keyword.Db2_on_Cloud_short}} sélectionnée.

## Démarrage du serveur MobileFirst
{: #start_mobilefoundation_p5}

* Pour démarrer le serveur
{{site.data.keyword.mfserver_short_notm}} avec les réglages par
défaut, cliquez sur **Démarrer le serveur de base**.

* Cette sélection met à disposition une instance {{site.data.keyword.mfserver_long_notm}} avec les réglages suivants :
    -  2 noeuds avec 1 Go de mémoire chacun. Cette taille
convient aux activités de développement et aux activités de test modérées, ainsi qu'aux charges
de travail de production à petite échelle.

    -	Le `nom d'utilisateur` et le `mot de passe` sont générés automatiquement pour vous. Vous pouvez y accéder une fois que le
serveur est en opération.

L'implantation de votre serveur débute. Ce processus prend environ 10 minutes et une fenêtre de
message indique la progression de l'opération. A son terme, un tableau de bord s'affiche et présente les éléments suivants :

  -	L'état du serveur que vous exécutez (état, taille).

  -	La route de serveur créée pour vous. Utilisez cette route dans votre application mobile pour vous connecter à {{site.data.keyword.mfserver_short_notm}}.

  -	Vos `nom d'utilisateur` et `mot de passe` personnels
pour accéder à la console {{site.data.keyword.mfp_oc_short_notm}}. Le `mot de passe` est masqué. Cliquez sur l'icône **Afficher le mot de passe** pour le visualiser.

*	Cliquez sur **Lancer la console** pour ouvrir la console {{site.data.keyword.mfp_oc_short_notm}}.


Elle vous permet de gérer vos applications, adaptateurs et
périphériques mobiles, ainsi que l'utilisation de votre serveur en
tant que serveur dorsal mobile, l'envoi de notifications push, etc.


##  Ajout d'un service Mobile Analytics
{: #adding_analytics_server_p5}

 Vous pouvez maintenant surveiller votre application mobile sur le serveur {{site.data.keyword.mobilefirst}} en ajoutant une instance de service
Mobile Analytics à l'instance {{site.data.keyword.mobilefoundation_short}}.

 <!--The Professional plan creates the Mobile Analytics service in a container group, the user can customize the configuration by selecting the number of container nodes in the container group.

 Users can also attach volumes to the containers to persist data. The volume once selected cannot be changed. 20 GB is the default file share space available to the user. If the user needs additional storage space to persist analytics data, he is required to buy additional file share and create a volume using this file share. He can then select this new volume while deploying the analytics server.

 For more information on adding volumes to {{site.data.keyword.containerlong}}, refer to [Storing persistent data in a volume by using the {{site.data.keyword.Bluemix_notm}} Dashboard ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.ng.bluemix.net/docs/containers/container_volumes_ov.html#container_volumes_ui.html){: new_window}.-->

* Cliquez sur **Ajouter un module d'analyse** pour créer et ajouter une instance de service
Mobile Analytics à l'instance {{site.data.keyword.mobilefoundation_short}}.

<!--* You can choose the Mobile Analytics service configuration, the minimum supported configuration for the Analytics server is 2 nodes with 1 GB memory each, you can choose to create an Analytics server up to a maximum configuration of 32 nodes with 16 GB memory each.-->

Le processus de mise à disposition commence. Ce processus prend environ 10 minutes. Vous pouvez suivre son avancement avec
l'indicateur de progression.  

* Lancez la console du service Mobile Analytics à partir de {{site.data.keyword.mfp_oc_short_notm}}.

* La connexion unique (SSO) est activée entre {{site.data.keyword.mfserver_short_notm}} et le service Mobile Analytics. Le service Mobile Analytics est configuré avec les mêmes clés LTPA et données d'identification d'utilisateur que celles du
serveur {{site.data.keyword.mfserver_short_notm}}. Cela signifie que, pour vous connecter à la console Mobile Analytics, vous pouvez utiliser les mêmes `nom d'utilisateur` et `mot de passe`
que ceux utilisés pour la connexion au serveur {{site.data.keyword.mfp_oc_short_notm}}.

Pour plus d'informations sur Mobile Analytics, vous pouvez vous référer
au site [MobileFirst Foundation Operational Analytics ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/analytics/){: new_window}.

> **Remarque :** La
suppression de l'instance de service {{site.data.keyword.mobilefoundation_short}} supprime l'instance de service Mobile Analytics.

##  Suppression du service Mobile Analytics
{: #deleting_analytics_server_p5}

Vous pouvez maintenant supprimer, à partir du tableau de bord du service {{site.data.keyword.mobilefoundation_short}}, le service
Mobile Analytics qui a été ajouté à l'instance de service {{site.data.keyword.mobilefoundation_short}}.

* Cliquez sur **Supprimer le module d'analyse** pour supprimer le service Mobile Analytics qui a été ajouté à l'instance de service
{{site.data.keyword.mobilefoundation_short}}.

 Le fait de cliquer sur **Supprimer le module d'analyse** supprime l'instance
de service Mobile Analytics. Cette opération prend environ 10 minutes. Vous pouvez actualiser l'écran pour examiner le
statut mis à jour. La suppression de l'instance Mobile Analytics
réactive le bouton **Ajouter un module d'analyse**. Utilisez ce bouton si vous souhaitez ajouter à nouveau le
service Mobile Analytics.

## Recréation du serveur MobileFirst
{: #recreate_mobilefoundation_p5}

*	Cliquez sur **Recréer** pour recréer le serveur.

* Cette action arrête votre serveur existant et supprime les données. Une
nouvelle instance de serveur est créée avec une version mise à jour, si elle est disponible. Cette action prend quelques minutes avant de s'achever.

> **Remarque** : Les
données de votre instance de serveur précédente, y compris les informations sur les applications et les adaptateurs,
sont conservées dans l'instance de service {{site.data.keyword.Db2_on_Cloud_short}} configurée. Elles sont utilisées pour recréer le serveur.

##	Mise en place d'une configuration avancée
{: #using_mfs_advanced_p5}

L'option **Démarrer le serveur avec la configuration avancée**
de la page `Présentation` permet de créer le serveur avec des réglages
avancés ou personnalisés. Vous pouvez également modifier les réglages du serveur pour personnaliser sa
configuration en cliquant sur l'onglet **Paramètres**. {{site.data.keyword.mobilefoundation_short}}
vous permet d'accéder à certains paramètres avancés.

*	Dans l'onglet **Topologie**, vous pouvez sélectionner la
taille du serveur, ainsi que le nombre d'instances dont vous avez besoin. Avec sa mémoire de 1 Go, le serveur par défaut
est suffisant pour le développement et des tests peu intensifs.
  - Sélectionnez la taille appropriée pour votre serveur compte tenu de
vos besoins.

  - La zone **Instances** affiche le nombre d'instances créées.

      <!--- {{site.data.keyword.mobilefirst}} server farm can be created by configuring the number of nodes here. The minimum supported configuration is 2 nodes with 1 GB memory each and the maximum supported configuration is 32 nodes with 16 GB memory each.-->

Pour plus de détails, consultez la documentation [{{site.data.keyword.mobilefoundation_long}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/support/knowledgecenter/SSHS8R_8.0.0/wl_welcome.html){: new_window}.
