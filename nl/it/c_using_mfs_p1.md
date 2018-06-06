---

copyright:
  years: 2016, 2018
lastupdated:  "2018-03-07"

---

#	Utilizzo del piano Developer
{: #using_mobilefoundation_p1}

Dopo aver creato l'istanza del servizio {{site.data.keyword.mobilefoundation_short}} utilizzando il piano Developer, accedi alla pagina della panoramica in {{site.data.keyword.Bluemix_notm}}. In questa pagina, ti vengono forniti i video e le esercitazioni per aiutarti a iniziare a utilizzare il servizio.

## Utilizzo del server MobileFirst
{: #start_mobilefoundation_p1}
* Puoi accedere e utilizzare immediatamente il server MobileFirst.

  Questa selezione fornisce un {{site.data.keyword.mfserver_long_notm}} con le seguenti impostazioni:
  *	1 GB di memoria. Questa dimensione è sufficiente per lo sviluppo, per delle attività leggere di test e i carichi di lavoro di produzione su piccola scala.

  * Per accedere al server MobileFirst utilizzando la CLI avrai bisogno delle credenziali, che sono disponibili quando fai clic su **Credenziali del servizio** dal pannello di navigazione di sinistra della console IBM Cloud.

<!--  The process of provisioning starts. This process takes about 10 minutes, and a message window indicates the progress of this operation. When complete a dashboard is displayed where you can see:
    *	The status of your server that is running (state, size).

    *	The server route created for you. Use this route in your mobile application to connect to the {{site.data.keyword.mfserver_short_notm}}.

    *	Your personal `username` and `password` to access the {{site.data.keyword.mfp_oc_short_notm}}. The `password` is hidden. Click **Show Password** icon to visualize it.

*	Click **Launch Console** to launch the {{site.data.keyword.mfp_oc_short_notm}}.-->

Puoi ora gestire le tue applicazioni mobili e i tuoi dispositivi mobili, utilizzare il tuo server come un backend mobile, inviare notifiche di push e altro ancora.

## Servizio Mobile Analytics
{: #adding_analytics_server_dev}

Il server Mobile Analytics è incluso e preconfigurato con l'istanza del servizio del piano Mobile Foundation: Developer.

<!-- You can now monitor your mobile application on {{site.data.keyword.mobilefirst}} server by adding a Mobile Analytics service to the {{site.data.keyword.mobilefoundation_short}} service instance. Developer plan creates the Mobile Analytics service in a container group with a single node having 1 GB memory.

* Click **Add Analytics** to add the Mobile Analytics service to the {{site.data.keyword.mobilefoundation_short}} service instance.

  The process of provisioning starts. This process takes about 10 minutes, and a message window indicates the progress of this operation.  -->

* Avvia la console del servizio Mobile Analytics da {{site.data.keyword.mfp_oc_short_notm}}.

* SSO (single sign-on) è abilitato tra {{site.data.keyword.mfserver_short_notm}} e il servizio Mobile Analytics. Il servizio Mobile Analytics è configurato con le stesse credenziali utente e chiavi LTPA di {{site.data.keyword.mfserver_short_notm}}. Puoi utilizzare gli stessi `nomeutente` e `password` per accedere alla console di Mobile Analytics utilizzati per accedere a {{site.data.keyword.mfp_oc_short_notm}}.

Per ulteriori informazioni su Mobile Analytics, puoi fare riferimento a [MobileFirst Foundation Operational Analytics ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/analytics/){: new_window}.

> **Nota:** l'eliminazione dell'istanza del servizio {{site.data.keyword.mobilefoundation_short}} rimuove l'istanza del servizio Mobile Analytics.

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

Fai riferimento alla documentazione di [{{site.data.keyword.mobilefoundation_long}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/support/knowledgecenter/SSHS8R_8.0.0/wl_welcome.html){: new_window} per ulteriori dettagli.
