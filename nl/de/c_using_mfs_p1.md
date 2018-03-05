---

copyright:
  years: 2016, 2018
lastupdated:  "2018-02-14"

---

#	Developer-Plan verwenden
{: #using_mobilefoundation_p1}

Nach der Erstellung der {{site.data.keyword.mobilefoundation_short}}-Serviceinstanz im Rahmen des Developer-Plans können Sie auf die Übersichtsseite in {{site.data.keyword.Bluemix_notm}} zugreifen. Auf dieser Seite finden Sie Lernprogramme und Videos, die Sie beim Einstieg in die Arbeit mit dem Service unterstützen.

## MobileFirst-Server starten
{: #start_mobilefoundation_p1}
* Um den {{site.data.keyword.mfserver_short_notm}} mit den Standardeinstellungen zu starten, klicken Sie auf **Basisserver starten**.

  Diese Auswahl stellt einen {{site.data.keyword.mfserver_long_notm}} mit den folgenden Einstellungen bereit:
  *	1 GB Hauptspeicher. Diese Größe ist für Entwicklungs- und kleinere Testaktivitäten sowie für kleinere Produktionsworkloads ausreichend.

  *	`Benutzername` und `Kennwort` werden automatisch für Sie generiert. Sie können darauf zugreifen, wenn der Server betriebsbereit ist.

  Der Prozess der Bereitstellung wird gestartet. Dieser Prozess dauert ungefähr 10 Minuten; in einem Nachrichtenfenster wird der Fortschritt dieser Operation angezeigt. Ist der Vorgang abgeschlossen, wird ein Dashboard mit folgenden Informationen angezeigt:
    *	Status Ihres Servers, der ausgeführt wird (Zustand, Größe).

    *	Für Sie erstellte Serverroute. Verwenden Sie diese Route in Ihrer mobilen Anwendung, um eine Verbindung zum {{site.data.keyword.mfserver_short_notm}} herzustellen.

    *	Ihr persönlicher `Benutzername` und das `Kennwort` für den Zugriff auf die {{site.data.keyword.mfp_oc_short_notm}}. Das `Kennwort` wird ausgeblendet. Klicken Sie auf das Symbol **Kennwort anzeigen**, um es einzublenden.

*	Klicken Sie auf **Konsole starten**, um die {{site.data.keyword.mfp_oc_short_notm}} zu starten.

Mit der Konsole können Sie Ihre mobilen Apps und Geräte verwalten, Ihren Server als mobiles Back-End verwenden, Push-Benachrichtigungen senden usw.

##  Mobile Analytics-Service hinzufügen
{: #adding_analytics_server_dev}

 Sie können Ihre mobile Anwendung nun auf dem {{site.data.keyword.mobilefirst}}-Server überwachen, indem Sie einen Mobile Analytics-Service zur {{site.data.keyword.mobilefoundation_short}}-Serviceinstanz hinzufügen. Durch den Entwicklerplan wird der Mobile Analytics-Service in einer Containergruppe mit einem einzigen Knoten und 1 GB Speicherplatz erstellt.

* Klicken Sie auf **Analytics hinzufügen**, um den Mobile Analytics-Service zur Instanz des {{site.data.keyword.mobilefoundation_short}}-Service hinzuzufügen.

  Der Prozess der Bereitstellung wird gestartet. Dieser Prozess dauert ungefähr 10 Minuten; in einem Nachrichtenfenster wird der Fortschritt dieser Operation angezeigt.  

* Starten Sie die Mobile Analytics-Servicekonsole über {{site.data.keyword.mfp_oc_short_notm}}.

* Für den {{site.data.keyword.mfserver_short_notm}} und den Mobile Analytics-Service ist Single Sign-on aktiviert. Der Mobile Analytics-Service ist mit denselben LTPA-Schlüsseln und denselben Benutzerberechtigungen konfiguriert wie der {{site.data.keyword.mfserver_short_notm}}. Sie können für die Anmeldung an der Mobile Analytics Console den `Benutzernamen` und das `Kennwort` verwenden, die Sie auch für die Anmeldung bei der {{site.data.keyword.mfp_oc_short_notm}} verwendet haben.

Weitere Informationen zu Mobile Analytics finden Sie unter [MobileFirst Foundation Operational Analytics ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/analytics/){: new_window}.

> **Hinweis:** Durch das Löschen der {{site.data.keyword.mobilefoundation_short}}-Serviceinstanz und die erneute Erstellung von {{site.data.keyword.mfserver_short_notm}} wird die Mobile Analytics-Serviceinstanz entfernt.

##  Mobile Analytics-Service löschen
{: #deleting_analytics_server_dev}

Sie können jetzt den Mobile Analytics-Service, der zur {{site.data.keyword.mobilefoundation_short}}-Serviceinstanz hinzugefügt wurde, im {{site.data.keyword.mobilefoundation_short}}-Service-Dashboard löschen.

* Klicken Sie auf die Option **Analytics löschen**, um den Mobile Analytics-Service zu löschen, der zur {{site.data.keyword.mobilefoundation_short}}-Serviceinstanz hinzugefügt wurde.

 Durch das Klicken auf **Analytics löschen** wird die Analytics-Serverinstanz gelöscht. Das Löschen der Analytics-Instanz dauert ca. 10 Minuten. Sie können die Anzeige aktualisieren, um den aktuellen Status anzuzeigen. Durch das Löschen der Analytics-Instanz wird die Schaltfläche **Analytics hinzufügen** erneut aktiviert. Wenn Sie den Mobile Analytics-Service erneut hinzufügen möchten, können Sie auf diese Schaltfläche klicken.


## MobileFirst-Server erneut erstellen
{: #recreate_mobilefoundation_p1}

*	Klicken Sie auf die Schaltfläche **Neu erstellen**, um den Server erneut zu erstellen.

* Diese Aktion stoppt Ihre vorhandenen Server und löscht die Daten. Alle Daten Ihres mobilen Servers gehen verloren. Eine neue Serverinstanz wird mit einer aktualisierten Version erstellt, falls verfügbar. Diese Aktion nimmt einige Minuten in Anspruch.

##	Erweiterte Konfiguration einrichten
{: #using_mfs_advanced_p1}

Mit der Option **Server mit erweiterter Konfiguration starten** auf der Seite `Übersicht` können Sie einen Server mit erweiterten oder benutzerdefinierten Einstellungen erstellen. Sie können die Servereinstellungen auch aktualisieren, um Ihre Serverkonfiguration anzupassen; klicken Sie hierfür auf die Registerkarte **Konfiguration**. {{site.data.keyword.mobilefoundation_short}} bietet Ihnen Zugriff auf einige erweiterte Einstellungen.

*	Auf der Registerkarte **Topologie** können Sie die Servergröße und die Anzahl der Instanzen auswählen, die den jeweiligen Anforderungen entsprechen. Die Standardgröße von 1 GB für den Server ist für die Entwicklung und kleinere Tests ausreichend.

  - Wählen Sie in Abhängigkeit von Ihrem Bedarf die richtige Größe für Ihren Server aus.

* **Knoten** zeigt die Anzahl der erstellten Knoten an. Dieses Feld ist nicht in {{site.data.keyword.mobilefoundation_short}}: Developer bearbeitbar. Die Anzahl der Knoten <!--in your {{site.data.keyword.IBM_notm}} container group--> beträgt standardmäßig **1** im Developer-Plan.

Weitere Details finden Sie in der [{{site.data.keyword.mobilefoundation_long}}-Dokumentation ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/support/knowledgecenter/SSHS8R_8.0.0/wl_welcome.html){: new_window}.
