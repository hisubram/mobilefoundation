---

copyright:
  years: 2016, 2018
lastupdated:  "2018-03-07"

---

#	Developer-Plan verwenden
{: #using_mobilefoundation_p1}

Nach der Erstellung der {{site.data.keyword.mobilefoundation_short}}-Serviceinstanz im Rahmen des Developer-Plans können Sie auf die Übersichtsseite in {{site.data.keyword.Bluemix_notm}} zugreifen. Auf dieser Seite finden Sie Lernprogramme und Videos, die Sie beim Einstieg in die Arbeit mit dem Service unterstützen.

## MobileFirst-Server verwenden
{: #start_mobilefoundation_p1}
* Sie können sofort auf den MobileFirst-Server zugreifen und diesen verwenden.

  Diese Auswahl stellt einen {{site.data.keyword.mfserver_long_notm}} mit den folgenden Einstellungen bereit:
  *	1 GB Hauptspeicher. Diese Größe ist für Entwicklungs- und kleinere Testaktivitäten sowie für kleinere Produktionsworkloads ausreichend.

  * Für den Zugriff auf den MobileFirst-Server über die Befehlszeilenschnittstelle (CLI) benötigen Sie die Berechtigungsnachweise, die zur Verfügung stehen, wenn Sie auf **Serviceberechtigungsnachweise** im linken Navigationsbereich der IBM Cloud-Konsole klicken.

<!--  The process of provisioning starts. This process takes about 10 minutes, and a message window indicates the progress of this operation. When complete a dashboard is displayed where you can see:
    *	The status of your server that is running (state, size).

    *	The server route created for you. Use this route in your mobile application to connect to the {{site.data.keyword.mfserver_short_notm}}.

    *	Your personal `username` and `password` to access the {{site.data.keyword.mfp_oc_short_notm}}. The `password` is hidden. Click **Show Password** icon to visualize it.

*	Click **Launch Console** to launch the {{site.data.keyword.mfp_oc_short_notm}}.-->

Sie können nun Ihre mobilen Apps und Geräte verwalten, Ihren Server als mobiles Back-End verwenden, Push-Benachrichtigungen senden usw.

## Mobile Analytics-Service
{: #adding_analytics_server_dev}

Mobile Analytics-Server ist in der Serviceinstanz des Mobile Foundation: Developer-Plans enthalten und vorkonfiguriert.

<!-- You can now monitor your mobile application on {{site.data.keyword.mobilefirst}} server by adding a Mobile Analytics service to the {{site.data.keyword.mobilefoundation_short}} service instance. Developer plan creates the Mobile Analytics service in a container group with a single node having 1 GB memory.

* Click **Add Analytics** to add the Mobile Analytics service to the {{site.data.keyword.mobilefoundation_short}} service instance.

  The process of provisioning starts. This process takes about 10 minutes, and a message window indicates the progress of this operation.  -->

* Starten Sie die Mobile Analytics-Servicekonsole über {{site.data.keyword.mfp_oc_short_notm}}.

* Für den {{site.data.keyword.mfserver_short_notm}} und den Mobile Analytics-Service ist Single Sign-on aktiviert. Der Mobile Analytics-Service ist mit denselben LTPA-Schlüsseln und denselben Benutzerberechtigungen konfiguriert wie der {{site.data.keyword.mfserver_short_notm}}. Sie können für die Anmeldung an der Mobile Analytics Console den `Benutzernamen` und das `Kennwort` verwenden, die Sie auch für die Anmeldung bei der {{site.data.keyword.mfp_oc_short_notm}} verwendet haben.

Weitere Informationen zu Mobile Analytics finden Sie unter [MobileFirst Foundation Operational Analytics ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/analytics/){: new_window}.

> **Hinweis:** Durch das Löschen der {{site.data.keyword.mobilefoundation_short}}-Serviceinstanz wird die Mobile Analytics-Serviceinstanz entfernt.

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

Weitere Details finden Sie in der [{{site.data.keyword.mobilefoundation_long}}-Dokumentation ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/support/knowledgecenter/SSHS8R_8.0.0/wl_welcome.html){: new_window}.
