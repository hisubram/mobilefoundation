---

copyright:
  years: 2018
lastupdated:  "2018-02-09"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}


# Häufige Fragen

Im Folgenden sind Antworten auf häufig gestellte Fragen im Zusammenhang mit dem {{site.data.keyword.mobilefoundation_long}}-Service aufgeführt.
{: shortdesc}

## Wie werde ich informiert, wenn Aktualisierungen für den {{site.data.keyword.mobilefoundation_short}}-Service verfügbar sind?
{: #maintupdates_mf}

{{site.data.keyword.mobilefoundation_short}} stellt einen {{site.data.keyword.mfserver_short_notm}} bereit. Die Benutzer werden über Aktualisierungen des {{site.data.keyword.mobilefoundation_short}}-Servers benachrichtigt. Die Benachrichtigung wird im Dashboard der Serviceinstanz angezeigt. Der Benutzer kann die Aktualisierung für {{site.data.keyword.mobilefoundation_short}} in einem von ihm festgelegten Wartungsfenster ausführen. Wenn es für Sie hilfreich ist, können Sie den {{site.data.keyword.mobilefoundation_short}}-Server aktualisieren.

Die {{site.data.keyword.mobilefoundation_short}}-Serviceaktualisierung steht zur Verfügung, wenn eine der folgenden Komponenten aktualisiert ist.

* {{site.data.keyword.mfserver_short_notm}}.
* Zugrunde liegende Liberty-Version.
* Zugrunde liegende Java Developer Kit-Version.

## Wie wende ich die Aktualisierungen für den {{site.data.keyword.mobilefoundation_short}}-Service an?
{: #apply_update_mf}

Die Aktualisierung für {{site.data.keyword.mobilefoundation_short}} kann durch Klicken auf die Option für die Neuerstellung angewendet werden.
Bei der Anwendung der Aktualisierung wird die Version des Servers, die in der {{site.data.keyword.mfp_oc_short_notm}} zu sehen ist, so geändert, dass die Version der Serveraktualisierung angezeigt wird.

> **Hinweis:**
>  * Benutzer können keine eigenen Fixes und Aktualisierungen auf ihre Instanz des {{site.data.keyword.mobilefoundation_short}}-Service anwenden.
>  * In [Server im Developer-Plan erneut erstellen](c_using_mfs_p1.html#recreate_mobilefoundation_p1), [Server im DeveloperPro-Plan erneut erstellen](c_using_mfs_p3.html#recreate_mobilefoundation_p3), [Server im Professional Per Device-Plan erneut erstellen](c_using_mfs_p4.html#recreate_mobilefoundation_p5) und [Server im Professional 1 Application-Plan](c_using_mfs_p2.html#recreate_mobilefoundation_p2) werden die Unterschiede im Verhalten beim Klicken auf **Erneut erstellen** für die jeweiligen Pläne beschrieben.
>

## Wie konfiguriere ich eine angepasste Domäne für meine {{site.data.keyword.mobilefoundation_short}}-Serverinstanz?
{: #configcustomdomain}

{{site.data.keyword.mobilefoundation_short}} stellt einen {{site.data.keyword.mfserver_short_notm}} bereit, auf den über eine URL zugegriffen werden kann, bei der die Domänennamen auf der {{site.data.keyword.Bluemix_notm}}-**Region** basieren. Sie können auch eine eigene benutzerdefinierte Domäne erstellen.

Bei der Erstellung der URL oder Route basieren die Standarddomänennamen auf der {{site.data.keyword.Bluemix_notm}}-`Region`.

  |Domäne |  Region  |    
  |:----- | :----- |    
  |`mybluemix.net` | US - Süden |    
  |`eu-gb.mybluemix.net` | Vereinigtes Königreich  |
  |`au-syd.mybluemix.net` | Sydney  |   
  |`eu-de.mybluemix.net` | Frankfurt |   
  {: caption="Tabelle 1. Anwendungsdomänennamen auf der Basis von Regionen in {{site.data.keyword.Bluemix_notm}}" caption-side="top"}

Zur Verwendung einer eigenen Domäne müssen Sie eine benutzerdefinierte Domäne konfigurieren, indem Sie die folgenden Schritte ausführen:

1.	Erstellen Sie eine {{site.data.keyword.mfserver_short_notm}}-Instanz, indem Sie die {{site.data.keyword.mobilefoundation_short}}-Serviceinstanz durch die Auswahl eines unterstützten Plans erstellen.

+ Fügen Sie die benutzerdefinierte Domäne, die Sie verwenden möchten, zu Ihrer {{site.data.keyword.Bluemix_notm}}-`Organisation` hinzu. Rufen Sie **Organisationen verwalten > Domänen > Domäne hinzufügen** auf, um eine eigene Domäne hinzuzufügen.

+ Legen Sie eine Route für die Verwendung der benutzerdefinierten Domäne durch den <!--container group--> Server fest.

+ Rufen Sie den DNS-Provider für Ihre Domäne auf und fügen Sie einen CNAME-Eintrag hinzu, der den Datenverkehr von Ihrer Domäne zur {{site.data.keyword.Bluemix_notm}}-Standardroute weiterleitet, unter der der <!--container group--> Server ausgeführt wird.

+ Wenn Sie für Ihre benutzerdefinierte Domäne `https` konfigurieren möchten, laden Sie das SSL-Zertifikat für Ihre Domäne in {{site.data.keyword.Bluemix_notm}} hoch. Rufen Sie hierzu **Organisationen verwalten > Domänen** auf, wählen Sie die benutzerdefinierte Domäne aus, für die das SSL-Zertifikat konfiguriert werden soll, und klicken Sie auf **Zertifikat hochladen**, um das SSL-Zertifikat für Ihre Domäne hochzuladen. Weitere Informationen finden Sie in [dieser Veröffentlichung ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://developer.ibm.com/bluemix/2014/09/28/ssl-certificates-bluemix-custom-domains/){: new_window}.
