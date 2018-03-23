---

copyright:
  years: 2016, 2018
lastupdated:  "2018-03-07"

---

#	Utilisation du plan Développeur
{: #using_mobilefoundation_p1}

Après avoir créé l'instance de service {{site.data.keyword.mobilefoundation_short}} utilisant le
plan Développeur, accédez à la page Présentation sur {{site.data.keyword.Bluemix_notm}}. Vous y trouverez des tutoriels et des vidéos pour vous aider à faire vos premiers pas avec le service.

## Utilisation du serveur MobileFirst
{: #start_mobilefoundation_p1}
* Vous pouvez instantanément accéder et utiliser le serveur MobileFirst.

  Cette sélection met à disposition une instance {{site.data.keyword.mfserver_long_notm}} avec les réglages suivants :
  *	1 Go de mémoire. Cette taille
suffit aux activités de développement et aux activités de test peu intensives, ainsi qu'aux charges
de travail de production à petite échelle.

  * Pour accéder au serveur MobileFirst à l'aide de l'interface de ligne de commande, vous aurez besoin de vos données d'identification, qui sont disponibles lorsque vous cliquez sur **Données d'identification pour le service** dans le panneau de navigation gauche de la console IBM Cloud.

<!--  The process of provisioning starts. This process takes about 10 minutes, and a message window indicates the progress of this operation. When complete a dashboard is displayed where you can see:
    *	The status of your server that is running (state, size).

    *	The server route created for you. Use this route in your mobile application to connect to the {{site.data.keyword.mfserver_short_notm}}.

    *	Your personal `username` and `password` to access the {{site.data.keyword.mfp_oc_short_notm}}. The `password` is hidden. Click **Show Password** icon to visualize it.

*	Click **Launch Console** to launch the {{site.data.keyword.mfp_oc_short_notm}}.-->

Vous pouvez maintenant gérer vos applications et périphériques mobiles, utiliser votre serveur en tant que serveur dorsal mobile, envoyer des notifications push, etc.

## Service Mobile Analytics
{: #adding_analytics_server_dev}

Le service Mobile Analytics est inclut et préconfiguré avec l'instance du plan de service Développeur Mobile Foundation.

<!-- You can now monitor your mobile application on {{site.data.keyword.mobilefirst}} server by adding a Mobile Analytics service to the {{site.data.keyword.mobilefoundation_short}} service instance. Developer plan creates the Mobile Analytics service in a container group with a single node having 1 GB memory.

* Click **Add Analytics** to add the Mobile Analytics service to the {{site.data.keyword.mobilefoundation_short}} service instance.

  The process of provisioning starts. This process takes about 10 minutes, and a message window indicates the progress of this operation.  -->

* Lancez la console du service Mobile Analytics à partir de {{site.data.keyword.mfp_oc_short_notm}}.

* La connexion unique (SSO) est activée entre {{site.data.keyword.mfserver_short_notm}} et le service Mobile Analytics. Le service Mobile Analytics est configuré avec les mêmes clés LTPA et données d'identification d'utilisateur que celles du
serveur {{site.data.keyword.mfserver_short_notm}}. Cela signifie que, pour vous connecter à la console Mobile Analytics, vous pouvez utiliser les mêmes `nom d'utilisateur` et `mot de passe`
que ceux utilisés pour la connexion au serveur {{site.data.keyword.mfp_oc_short_notm}}.

Pour plus d'informations sur Mobile Analytics, vous pouvez vous référer
au site [MobileFirst Foundation Operational Analytics ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/analytics/){: new_window}.

> **Remarque :** La
suppression de l'instance de service {{site.data.keyword.mobilefoundation_short}} supprime l'instance de service Mobile Analytics.

<!--##  Deleting Mobile Analytics service
{: #deleting_analytics_server_dev}

You can now delete the Mobile Analytics service that was added to the {{site.data.keyword.mobilefoundation_short}} service instance, from the {{site.data.keyword.mobilefoundation_short}} service dashboard.

* Click **Delete Analytics** to delete the  Mobile Analytics service that was added to the {{site.data.keyword.mobilefoundation_short}} service instance.

 Clicking **Delete Analytics** deletes the analytics server instance. The process of deleting analytics instance takes about 10 minutes. You can refresh the screen to view the updated status. Deletion of analytics instance reenables the **Add Analytics** button. If you choose to add the Mobile Analytics service again, you can click this button.


## Re-creating the MobileFirst server
{: #recreate_mobilefoundation_p1}

*	Click **Recreate** to re-create the server.

* This action stops your existing server and deletes the data. All the data in your mobile server is lost. A new server instance is created with an updated version, if available. This action takes a few minutes to complete.

##	Setting up advanced configuration
{: #using_mfs_advanced_p1}

Use the **Start Server with Advanced Configuration** from the `Overview` page to create the server with advanced or custom settings. You can also update the server settings to customize your server configuration by clicking the **Configuration** tab. {{site.data.keyword.mobilefoundation_short}} gives you access to some advanced settings.

*	From the **Topology** tab, you can select the server size and the number of instances you need. The default 1 GB server is enough for development and moderate testing.

  - Select the correct size for your server based on your need.

* **Nodes** displays the number of nodes that are created. This field is not editable in {{site.data.keyword.mobilefoundation_short}}: Developer. The number of nodes is defaulted to **1** in the Developer plan.-->

Pour plus de détails, consultez la documentation [{{site.data.keyword.mobilefoundation_long}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/support/knowledgecenter/SSHS8R_8.0.0/wl_welcome.html){: new_window}.
