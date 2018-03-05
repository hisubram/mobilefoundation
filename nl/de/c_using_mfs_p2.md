---

copyright:
  years: 2016, 2018
lastupdated:  "2018-02-14"

---

#	Professional 1 Application-Plan verwenden
{: #using_mobilefoundation_p2}

Mit dem Professional 1 Application-Plan können Benutzer 1 mobile Anwendung mit mehreren Betriebssystemen für mobile Geräte erstellen.
Lesen Sie nach der Erstellung der Serviceinstanz von {{site.data.keyword.mobilefoundation_short}}: Professional 1 Application die folgende Prozedur, um die Arbeit mit dem Service zu beginnen.

## Voraussetzungen
{: #prerequisites_p2}

Beachten Sie Folgendes, bevor Sie die Serviceinstanz von {{site.data.keyword.mobilefoundation_short}}: Professional 1 Application konfigurieren.
* {{site.data.keyword.mobilefoundation_short}}: Professional 1 Application wird nur mit {{site.data.keyword.Bluemix_notm}}-Plänen für {{site.data.keyword.Db2_on_Cloud_short}} unterstützt.

* Sie benötigen Zugriff auf die Berechtigungsnachweise der {{site.data.keyword.Db2_on_Cloud_short}}-Serviceinstanz, bevor Sie die Einstellungen für die {{site.data.keyword.mobilefoundation_short}}-Serviceinstanz konfigurieren.

> **Hinweis**: Die {{site.data.keyword.Db2_on_Cloud_short}}-Serviceinstanz kann sich in jedem `Bereich` in Ihrer {{site.data.keyword.Bluemix_notm}}-`Organisation` bzw. in jeder anderen `Organisation`, auf die Sie zugreifen können, befinden. Stellen Sie sicher, dass Sie über die Berechtigungen für den Zugriff auf den `Bereich` verfügen, in dem sich die {{site.data.keyword.Db2_on_Cloud_short}}-Serviceinstanz befindet.


## Datenbankverbindung hinzufügen
{: #configure_dashdb_p2}

###  Erste Schritte
{: #firststeps_p2}

Führen Sie nach der Erstellung der Serviceinstanz von {{site.data.keyword.mobilefoundation_short}}: Professional 1 Application die beschriebene Prozedur aus, um mit der Verwendung des Service zu beginnen.

### Verbindung zur Db2 on Cloud-Serviceinstanz einrichten
{: #connect_dashdb_p2}

Nachdem die {{site.data.keyword.mobilefoundation_short}}: Professional 1 Application-Serviceinstanz erstellt wurde, wird die Seite *Übersicht* angezeigt, auf der Sie die Verbindungsinformationen für die {{site.data.keyword.Db2_on_Cloud_short}}-Serviceinstanz angeben müssen, zu der die {{site.data.keyword.mobilefoundation_short}}-Serviceinstanz eine Verbindung herstellen soll.

Sie können auch eine neue Serviceinstanz von {{site.data.keyword.Db2_on_Cloud_short}} erstellen, falls noch keine vorhanden ist.

Führen Sie die folgenden Schritte aus, um eine neue Db2 on Cloud-Serviceinstanz zu erstellen:

1. Wählen Sie auf der Seite *Übersicht* den Abschnitt **Neuen Service erstellen** aus.

+ Wählen Sie `Ja` für die Option **Hochverfügbarkeitskonfiguration** aus, wenn Sie eine {{site.data.keyword.Db2_on_Cloud_short}}-Serviceinstanz mit Hochverfügbarkeitsfunktionalität benötigen.

+ Prüfen Sie die Plandetails und klicken Sie auf **Erstellen**.

Eine neue {{site.data.keyword.Db2_on_Cloud_short}}-Serviceinstanz wird erstellt. Mit dieser steht eine dedizierte {{site.data.keyword.Db2_on_Cloud_short}}-Instanz mit 8 GB RAM und 2 vCPUs sowie 500 GB Speicher zur Verfügung.

Führen Sie die folgenden Schritte aus, um die Verbindung zu einer vorhandenen {{site.data.keyword.Db2_on_Cloud_short}}-Serviceinstanz oder zu der Serviceinstanz von {{site.data.keyword.Db2_on_Cloud_short}} herzustellen, die Sie gerade erstellt haben:

1. Wählen Sie die {{site.data.keyword.Bluemix_notm}} `Organisation` aus, in der sich die {{site.data.keyword.Db2_on_Cloud_short}}-Serviceinstanz befindet.

+ Wählen Sie den {{site.data.keyword.Bluemix_notm}}-`Bereich`, in dem sich die {{site.data.keyword.Db2_on_Cloud_short}}-Serviceinstanz befindet, in der Liste der Bereiche aus, die in der ausgewählten `Organisation` verfügbar sind.   
> **Hinweis:** Wenn die `Organisation` und der `Bereich`, in denen sich Ihre {{site.data.keyword.Db2_on_Cloud_short}}-Serviceinstanz befindet, nicht aufgeführt sind, prüfen Sie, ob Sie ein Mitglied der betreffenden `Organisation` bzw. des betreffenden `Bereichs` sind. Sie benötigen die Zugriffsrolle *Entwickler* für die Organisation und den Bereich, da der {{site.data.keyword.mobilefoundation_short}}-Service über den {{site.data.keyword.Db2_on_Cloud_short}}-Service auf die Berechtigungsnachweise zugreift.

+ Wählen Sie den `Servicenamen` und die `Berechtigungsnachweise` für {{site.data.keyword.Db2_on_Cloud_short}} aus, um eine Verbindung zur vorhandenen {{site.data.keyword.Db2_on_Cloud_short}}-Serviceinstanz herzustellen.

+  Testen Sie die Verbindung zur angegebenen {{site.data.keyword.Db2_on_Cloud_short}}-Serviceinstanz.

+  Klicken Sie auf **Hinzufügen**. Mit dieser Aktion werden die erforderlichen Tabellen in der konfigurierten {{site.data.keyword.Db2_on_Cloud_short}}-Datenbankserviceinstanz erstellt.

Nach einigen Sekunden können Sie auf die Seite `Übersicht` zugreifen, auf der die Lernprogramme und Videos zur Verfügung stehen, die Sie beim Einstieg in die Verwendung des {{site.data.keyword.mobilefoundation_short}}-Service unterstützten.

> **Hinweis**: Sie können die {{site.data.keyword.Db2_on_Cloud_short}}-Serviceinstanz, die zur Verwendung durch die {{site.data.keyword.mobilefoundation_short}}-Serviceinstanz konfiguriert ist, nicht ändern. Sie können jedoch dieselbe {{site.data.keyword.Db2_on_Cloud_short}}-Serviceinstanz für mehrere {{site.data.keyword.mobilefoundation_short}}-Serviceinstanzen verwenden, da jede {{site.data.keyword.mobilefoundation_short}}-Serviceinstanz ein eigenes Schema in der ausgewählten {{site.data.keyword.Db2_on_Cloud_short}}-Serviceinstanz erstellt.


## MobileFirst-Server starten
{: #start_mobilefoundation_p2}

* Um den {{site.data.keyword.mfserver_short_notm}} mit den Standardeinstellungen zu starten, klicken Sie auf **Basisserver starten**.

* Diese Auswahl stellt einen {{site.data.keyword.mfserver_long_notm}} mit den folgenden Einstellungen bereit:
    -  1 GB Hauptspeicher. Diese Größe ist für Entwicklungs- und kleinere Testaktivitäten sowie für kleinere Produktionsworkloads ausreichend.

    -	`Benutzername` und `Kennwort` werden automatisch für Sie generiert. Sie können darauf zugreifen, wenn der Server betriebsbereit ist.

    Der Prozess der Bereitstellung Ihres Servers wird gestartet. Dieser Prozess dauert ungefähr 10 Minuten; in einem Nachrichtenfenster wird der Fortschritt dieser Operation angezeigt. Ist der Vorgang abgeschlossen, wird ein Dashboard mit folgenden Informationen angezeigt:

      -	Status Ihres Servers, der ausgeführt wird (Zustand, Größe).

      -	Für Sie erstellte Serverroute. Verwenden Sie diese Route in Ihrer mobilen Anwendung, um eine Verbindung zum {{site.data.keyword.mfserver_short_notm}} herzustellen.

      -	Ihr persönlicher `Benutzername` und das `Kennwort` für den Zugriff auf die {{site.data.keyword.mfp_oc_short_notm}}. Das `Kennwort` wird ausgeblendet. Klicken Sie auf das Symbol **Kennwort anzeigen**, um es einzublenden.

*	Klicken Sie auf **Konsole starten**, um die {{site.data.keyword.mfp_oc_short_notm}} zu öffnen.

Mit der Konsole können Sie Ihre mobilen Apps, Adapter und Geräte verwalten, Ihren Server als mobiles Back-End verwenden, Push-Benachrichtigungen senden usw.

##  Mobile Analytics-Service hinzufügen
{: #adding_analytics_server_prof}

 Sie können Ihre mobile Anwendung nun auf dem {{site.data.keyword.mobilefirst}}-Server überwachen, indem Sie einen Mobile Analytics-Service zur {{site.data.keyword.mobilefoundation_short}}-Serviceinstanz hinzufügen.

 Eine Mobile Analytics-Serviceinstanz wird mit dem Professional-Plan erstellt.

 <!--Users can also attach volumes to the containers to persist data. The volume once selected cannot be changed. 20 GB is the default file share space available to the user. If the user needs additional storage space to persist analytics data, he is required to buy additional file share and create a volume using this file share. He can then select this new volume while deploying the analytics server.

 For more information on adding volumes to {{site.data.keyword.containerlong}}, refer to [Storing persistent data in a volume by using the {{site.data.keyword.Bluemix_notm}} Dashboard ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.ng.bluemix.net/docs/containers/container_volumes_ov.html#container_volumes_ui){: new_window}.-->

 * Klicken Sie auf **Analytics hinzufügen**, um den Mobile Analytics-Service zur Instanz des {{site.data.keyword.mobilefoundation_short}}-Service hinzuzufügen.

   Der Prozess der Bereitstellung wird gestartet. Dieser Prozess nimmt einige Minuten in Anspruch; mit einem Fortschrittsanzeiger wird der Verarbeitungsfortschritt dieser Operation angezeigt.  

 * Starten Sie die Mobile Analytics-Servicekonsole über {{site.data.keyword.mfp_oc_short_notm}}.

 * Für den {{site.data.keyword.mfserver_short_notm}} und den Mobile Analytics-Service ist Single Sign-on aktiviert. Der Mobile Analytics-Service ist mit denselben LTPA-Schlüsseln und denselben Benutzerberechtigungen konfiguriert wie der {{site.data.keyword.mfserver_short_notm}}-Server. Sie können für die Anmeldung an der Mobile Analytics Console den `Benutzernamen` und das `Kennwort` verwenden, die Sie auch für die Anmeldung bei der {{site.data.keyword.mfp_oc_short_notm}} verwendet haben.

 Weitere Informationen zu Mobile Analytics finden Sie in [MobileFirst Foundation Operational Analytics ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/analytics/){: new_window}.

> **Hinweis:** Durch das Löschen der {{site.data.keyword.mobilefoundation_short}}-Serviceinstanz wird die Mobile Analytics-Serviceinstanz entfernt.

##  Mobile Analytics-Service löschen
{: #deleting_analytics_server_prof}

Sie können jetzt den Mobile Analytics-Service, der zur {{site.data.keyword.mobilefoundation_short}}-Serviceinstanz hinzugefügt wurde, im {{site.data.keyword.mobilefoundation_short}}-Service-Dashboard löschen.

* Klicken Sie auf die Option **Analytics löschen**, um den Mobile Analytics-Service zu löschen, der zur {{site.data.keyword.mobilefoundation_short}}-Serviceinstanz hinzugefügt wurde.

 Durch das Klicken auf **Analytics löschen** wird die Analytics-Serverinstanz gelöscht. Das Löschen der Analytics-Instanz dauert ca. 10 Minuten. Sie können die Anzeige aktualisieren, um den aktuellen Status anzuzeigen. Durch das Löschen der Analytics-Instanz wird die Schaltfläche **Analytics hinzufügen** erneut aktiviert. Wenn Sie den Mobile Analytics-Service erneut hinzufügen möchten, können Sie auf diese Schaltfläche klicken.


## MobileFirst-Server erneut erstellen
{: #recreate_mobilefoundation_p2}

*	Klicken Sie auf die Schaltfläche **Neu erstellen**, um den Server erneut zu erstellen.

* Diese Aktion stoppt Ihre vorhandenen Server und löscht die Daten. Eine neue Serverinstanz wird mit einer aktualisierten Version erstellt, falls verfügbar. Diese Aktion nimmt einige Minuten in Anspruch.

> **Hinweis**: Daten der vorherigen Serverinstanz, einschließlich Informationen zu den Apps und Adaptern, werden in der konfigurierten {{site.data.keyword.Db2_on_Cloud_short}}-Serviceinstanz beibehalten. Diese Daten werden zur erneuten Erstellung des Servers verwendet.

##	Erweiterte Konfiguration einrichten
{: #using_mfs_advanced_p2}

Mit der Option **Server mit erweiterter Konfiguration starten** auf der Seite `Übersicht` können Sie einen Server mit erweiterten oder benutzerdefinierten Einstellungen erstellen. Sie können die Servereinstellungen auch aktualisieren, um Ihre Serverkonfiguration anzupassen; klicken Sie hierfür auf die Registerkarte **Konfiguration**. {{site.data.keyword.mobilefoundation_short}} bietet Ihnen Zugriff auf einige erweiterte Einstellungen.

*	Auf der Registerkarte **Topologie** können Sie die Servergröße und die Anzahl der Serverinstanzen auswählen, die den jeweiligen Anforderungen entsprechen. Die Standardgröße von 1 GB für den Server ist für die Entwicklung und kleinere Tests ausreichend.
  - Wählen Sie in Abhängigkeit von Ihrem Bedarf die richtige Größe für Ihren Server aus.

  - **Instanzen** zeigt die Anzahl der erstellten Knoten an. 

      <!--- {{site.data.keyword.mobilefirst}} server farm can be created by configuring the number of nodes here.-->

Weitere Details finden Sie in der [{{site.data.keyword.mobilefoundation_long}}-Dokumentation ![Symbol für externen Link icon](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/support/knowledgecenter/SSHS8R_8.0.0/wl_welcome.html){: new_window}.
