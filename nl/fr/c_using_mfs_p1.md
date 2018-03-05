---

copyright:
  years: 2016, 2018
lastupdated:  "2018-02-14"

---

#	Utilisation du plan Developer
{: #using_mobilefoundation_p1}

Après avoir créé l'instance de service {{site.data.keyword.mobilefoundation_short}} utilisant le
plan Developer, accédez à la page Présentation sur {{site.data.keyword.Bluemix_notm}}.
Vous y trouverez des tutoriels et des vidéos pour vous aider à faire vos premiers pas avec le service.


## Démarrage du serveur MobileFirst
{: #start_mobilefoundation_p1}
* Pour démarrer le serveur
{{site.data.keyword.mfserver_short_notm}} avec les réglages par
défaut, cliquez sur **Démarrer le serveur de base**.

  Cette sélection met à disposition une instance {{site.data.keyword.mfserver_long_notm}} avec les réglages suivants :
  *	1 Go de mémoire. Cette taille
suffit aux activités de développement et aux activités de test peu intensives, ainsi qu'aux charges
de travail de production à petite échelle.


  *	Le `nom d'utilisateur` et le `mot de passe` sont générés automatiquement pour vous.
Vous pouvez y accéder une fois que le
serveur est en opération.

  Le processus de mise à disposition commence. Ce processus prend environ 10 minutes et une fenêtre de
message indique la progression de l'opération. A son terme, un tableau de bord s'affiche et présente les éléments suivants :
    *	L'état du serveur que vous exécutez (état, taille).

    *	La route de serveur créée pour vous. Utilisez cette route dans votre application mobile pour vous connecter à {{site.data.keyword.mfserver_short_notm}}.

    *	Vos `nom d'utilisateur` et `mot de passe` personnels
pour accéder à la console {{site.data.keyword.mfp_oc_short_notm}}.
Le `mot de passe` est masqué.
Cliquez sur l'icône **Afficher le mot de passe** pour le visualiser.

*	Cliquez sur **Lancer la console** pour lancer la
console {{site.data.keyword.mfp_oc_short_notm}}.

Elle vous permet de gérer vos
applications et périphériques mobiles, l'utilisation de votre serveur en tant
que serveur dorsal mobile, l'envoi de notifications push, etc.

##  Ajout d'un service Mobile Analytics
{: #adding_analytics_server_dev}

 Vous pouvez maintenant surveiller votre application mobile sur le serveur {{site.data.keyword.mobilefirst}} en ajoutant un service
Mobile Analytics à l'instance de service {{site.data.keyword.mobilefoundation_short}}.
Le plan Developer crée le service Mobile Analytics dans un groupe de conteneurs avec un seul noeud d'1 Go de mémoire.

* Cliquez sur **Ajouter un module d'analyse** pour ajouter le service Mobile Analytics à l'instance de service {{site.data.keyword.mobilefoundation_short}}.

  Le processus de mise à disposition commence. Ce processus prend environ 10 minutes et une fenêtre de
message indique la progression de l'opération.  

* Lancez la console du service Mobile Analytics à partir de {{site.data.keyword.mfp_oc_short_notm}}.

* La connexion unique (SSO) est activée entre {{site.data.keyword.mfserver_short_notm}} et le service Mobile Analytics.
Le service Mobile Analytics est configuré avec les mêmes clés LTPA et données d'identification d'utilisateur que celles du
serveur {{site.data.keyword.mfserver_short_notm}}.
Cela signifie que, pour vous connecter à la console Mobile Analytics, vous pouvez utiliser les mêmes `nom d'utilisateur` et `mot de passe`
que ceux utilisés pour la connexion au serveur {{site.data.keyword.mfp_oc_short_notm}}.

Pour plus d'informations sur Mobile Analytics, vous pouvez vous référer
au site [MobileFirst Foundation Operational Analytics ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/analytics/){: new_window}.

> **Remarque :** La
suppression de l'instance de service {{site.data.keyword.mobilefoundation_short}} ou la recréation
du serveur {{site.data.keyword.mfserver_short_notm}} supprime l'instance de service Mobile Analytics.


##  Suppression du service Mobile Analytics
{: #deleting_analytics_server_dev}

Vous pouvez maintenant supprimer, à partir du tableau de bord du service {{site.data.keyword.mobilefoundation_short}}, le service
Mobile Analytics qui a été ajouté à l'instance de service {{site.data.keyword.mobilefoundation_short}}.


* Cliquez sur **Supprimer le module d'analyse** pour supprimer le service Mobile Analytics qui a été ajouté à l'instance de service
{{site.data.keyword.mobilefoundation_short}}.

 Le fait de cliquer sur **Supprimer le module d'analyse** supprime l'instance
de service Mobile Analytics.
Cette opération prend environ 10 minutes. Vous pouvez actualiser l'écran pour examiner le
statut mis à jour. La suppression de l'instance Mobile Analytics
réactive le bouton **Ajouter un module d'analyse**.
Utilisez ce bouton si vous souhaitez ajouter à nouveau le
service Mobile Analytics.



## Recréation du serveur MobileFirst
{: #recreate_mobilefoundation_p1}

*	Cliquez sur **Recréer** pour recréer le serveur.

* Cette action arrête votre serveur existant et supprime les données. Toutes les
données de votre serveur mobile sont perdues. Une
nouvelle instance de serveur est créée avec une version mise à jour, si elle est disponible. Cette action prend quelques minutes avant de s'achever.

##	Mise en place d'une configuration avancée
{: #using_mfs_advanced_p1}

L'option **Démarrer le serveur avec la configuration avancée**
de la page `Présentation` permet de créer le serveur avec des réglages
avancés ou personnalisés.
Vous pouvez également modifier les réglages du serveur pour personnaliser sa
configuration en cliquant sur l'onglet **Configuration**.
{{site.data.keyword.mobilefoundation_short}}
vous permet d'accéder à certains paramètres avancés.

*	Dans l'onglet **Topologie**, vous pouvez sélectionner la
taille du serveur, ainsi que le nombre d'instances dont vous avez besoin. Avec sa mémoire de 1 Go, le serveur par défaut
est suffisant pour le développement et des tests sommaires.

  - Sélectionnez la taille appropriée pour votre serveur compte tenu de
vos besoins.

* La zone **Noeuds** affiche le nombre de noeuds créés. Cette
zone n'est pas modifiable dans {{site.data.keyword.mobilefoundation_short}}: Developer. Le nombre de noeuds <!--in your {{site.data.keyword.IBM_notm}} container group--> est de **1** par défaut dans le plan Developer.

Pour plus de détails, consultez la documentation [{{site.data.keyword.mobilefoundation_long}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/support/knowledgecenter/SSHS8R_8.0.0/wl_welcome.html){: new_window}.
