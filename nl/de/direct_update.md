---

copyright:
  years: 2018
lastupdated:  "2018-05-11"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

#	Direct Update in Cordova-Anwendungen
{: #direct_update_cordova_apps}

Webressourcen (JavaScript, HTML, CSS oder Bilddateien) in Cordova-Anwendungen können mit Direct Update *drahtlos (over-the-air, OTA)* übertragen werden. Durch Verwendung der Direct Update-Funktion können Unternehmen sicherstellen, dass die Endbenutzer die jeweils aktuelle Version ihrer Apps nutzen.
Um eine Anwendung zu aktualisieren, müssen die aktualisierten Webressourcen der Anwendung mithilfe der MobileFirst-CLI oder durch Implementieren einer generierten Archivdatei gepackt und an den MobileFirst-Server hochgeladen werden. Direct Update wird darauf automatisch aktiviert. Danach muss es bei jeder Benutzeranforderung für eine geschützte Ressource erzwungen werden.

Direct Update wird auf der Cordova iOS- und der Cordova Android-Plattform unterstützt.

Zu Entwicklungs- und Testzwecken verwenden Entwickler typischerweise Direct Update, indem sie einfach ein Archiv auf den Entwicklungsserver hochladen. Dieser Prozess ist zwar leicht zu implementieren, aber er ist nicht sicher. Für diese Phase wird ein internes RSA-Schlüsselpaar verwendet, das aus einem in MobileFirst integrierten selbst signierten Zertifikat extrahiert wird.

Für die Live-Produktion oder die Vorproduktions-Testphase jedoch wird empfohlen, Secure Direct Update zu implementieren, bevor Sie Ihre Anwendung im App Store veröffentlichen. Für Secure Direct Update ist ein RSA-Schlüsselpaar erforderlich, welches aus einem reellen signierten Serverzertifikat extrahiert wird.

* Achten Sie darauf, die Keystore-Konfiguration nicht mehr zu ändern, nachdem die Anwendung veröffentlicht ist. Heruntergeladene Aktualisierungen können erst authentifiziert werden, nachdem die Anwendung mit einem neuen öffentlichen Schlüssel rekonfiguriert und erneut veröffentlicht wurde. Werden die zwei obigen Schritte nicht ausgeführt, schlägt Direct Update beim Client fehl.
* Mit Direct Update werden nur die Webressourcen der Anwendung aktualisiert. Um native Ressourcen zu aktualisieren, muss eine neue Anwendungsversion an das jeweilige App Store übergeben werden.
* Wenn Sie die Funktion des Direct Update nutzen und die Prüfsummen-Funktion für Webressourcen aktiviert ist, wird mit jedem Direct Update eine neue Kontrollsummenbasis eingerichtet.
* Wurde der MobileFirst-Server unter Verwendung eines Fixpacks aufgerüstet, bedient er Direct Updates weiterhin ordnungsgemäß. Wurde jedoch ein erst kürzlich erstelltes Direct Update-Archiv (ZIP-Datei) hochgeladen, kann dieses Aktualisierungen an älteren Clients stoppen. Der Grund dafür ist, dass das Archiv die Version des `cordova-plugin-mfp`-Plug-ins enthält. Bevor er mit diesem Archiv einen mobilen Client bedient, vergleicht der Server die Clientversion mit der Plug-in-Version. Liegen die beiden Versionen nahe genug beieinander (d. h. die drei wichtigsten Ziffern sind identisch), wird Direct Update normal durchgeführt. Andernfalls überspringt der MobileFirst-Server im Hintergrund die Aktualisierung. Eine Lösung für die Versionsabweichung ist das Herunterladen von `cordova-plugin-mfp` mit derselben Version wie die in Ihrem ursprünglichen Cordova-Projekt und das Neugenerieren des Direct Update-Archivs.
* Unter optimalen Bedingungen kann ein einzelner MobileFirst-Server Daten per Push-Operation an Clients mit einer Rate von 250 MB pro Sekunde übertragen. Sind höhere Raten erforderlich, sollte ein Cluster oder ein CDN-Service in Erwägung gezogen werden.
{: tip}

## Wie funktioniert Direct Update?
{: #working_direct_update}

Die Anwendungswebressourcen werden zunächst mit der Anwendung gepackt, um eine erste Offline-Verfügbarkeit zu gewährleisten. Danach prüft die Anwendung bei jeder Anforderung an den MobileFirst-Server auf Aktualisierungen.

>**Hinweis:** Nach der Durchführung eines Direct Update wird nach 60 Minuten erneut geprüft.

![Diagramm für die Funktionsweise von Direct Update](images/internal_function.jpg)

Nach einem Direct Update verwendet die Anwendung die vorgepackten Webressourcen nicht mehr. Stattdessen verwendet sie die heruntergeladenen Webressourcen aus der Sandbox der Anwendung. Wird der Cache der Anwendung im Gerät gelöscht, werden die ursprünglichen gepackten Webressourcen erneut verwendet.

>Ein Direct Update wird nur auf eine bestimmte Version angewendet. Mit anderen Worten, Aktualisierungen, die für eine Anwendung der Version 2.0 generiert wurden, können nicht auf eine andere Version derselben Anwendung angewandt werden.

## Aktualisierte Webressourcen erstellen und implementieren
{: #creating_deploying_updates}

Die aktualisierten Webressourcen müssen gepackt und an den MobileFirst-Server hochgeladen werden.

1. Öffnen Sie ein Befehlszeilenfenster und navigieren Sie zum Stamm des Cordova-Projekts.
2. Führen Sie den Befehl aus:
  ```
  mfpdev app webupdate
  ```
  {: pre}
Der Befehl `mfpdev app webupdate` packt die aktualisierten Webressourcen in eine `.zip`-Datei und lädt diese an den Standard-MobileFirst-Server hoch, der in der Entwicklerworkstation ausgeführt wird. Die gepackten Webressourcen sind im Ordner `[cordova-project-root-folder]/mobilefirst/` zu finden.

**Alternative Schritte:**

* Erstellen Sie die `.zip`-Datei und laden Sie sie an einen anderen MobileFirst-Server hoch: `mfpdev app webupdate [server-name] [runtime-name]`.
  Beispiel:
  ```
  mfpdev app webupdate myQAServer MyBankApps
  ```
  {: pre}

* Laden Sie eine zuvor generierte `.zip`-Datei hoch: `mfpdev app webupdate [server-name] [runtime-name] --file [path-to-packaged-web-resources]`.
  Beispiel:
  ```
  mfpdev app webupdate myQAServer MyBankApps --file mobilefirst/ios/com.mfp.myBankApp-1.0.1.zip
  ```
  {: pre}

* Laden Sie manuell gepackte Webressourcen an den MobileFirst-Server hoch:
  1. Erstellen Sie die .zip-Datei, ohne sie hochzuladen:
      ```
      mfpdev app webupdate --build
      ```
      {: pre}
  2. Laden Sie die MobileFirst Operations Console und klicken Sie auf den Anwendungseintrag.
  3. Klicken Sie auf **Webressourcendatei hochladen**, um die gepackten Webressourcen hochzuladen.    
      ![ZIP-Datei für Direct Update aus der Konsole hochladen](images/upload-direct-update-package.png)

Führen Sie den Befehl `mfpdev help app webupdate` aus, um weitere Informationen zu erhalten.
{: tip}

## Benutzererfahrung
{: #user_experience}

Nach dem Empfang eines Direct Update wird standardmäßig ein Dialog angezeigt, in dem der Benutzer nach seiner Genehmigung für den Aktualisierungsprozess gefragt wird. Nach der Genehmigung durch den Benutzer wird ein Dialog mit einer Fortschrittsleiste angezeigt und die Webressourcen werden heruntergeladen. Nach beendeter Aktualisierung wird die Anwendung automatisch neu geladen.

![Beispiel für Direct Update](images/direct-update-flow.png)

## Direct Update-Benutzerschnittstelle anpassen
{: #customize_du_ui}

Die dem Endbenutzer dargestellte Direct Update-Benutzerschnittstelle kann angepasst werden.
Fügen Sie innerhalb der Funktion `wlCommonInit()` in **index.js** Folgendes hinzu:
```JavaScript
wl_DirectUpdateChallengeHandler.handleDirectUpdate = function(directUpdateData, directUpdateContext) {
    // Angepasste Logik für Direct Update implementieren
};
```
{: codeblock}

*directUpdateData* ist ein JSON-Objekt, das die Eigenschaft 'downloadSize' enthält, die die Dateigröße (in Byte) des vom MobileFirst-Server herunterzuladenden Aktualisierungspakets darstellt.
*directUpdateContext* ist ein JavaScript-Objekt, das die Funktionen .start() und .stop() verfügbar macht, mit denen der Ablauf von Direct Update gestartet und gestoppt wird.

Sind die Webressourcen auf dem MobileFirst-Server neuer als in der Anwendung, werden Abfragedaten von Direct Update zur Serverantwort hinzugefügt. Jedes Mal, wenn das clientseitige MobileFirst-Framework diese Direct Update-Abfrage erkennt, ruft es die Funktion `wl_directUpdateChallengeHandler.handleDirectUpdate` auf.

Die Funktion bietet ein Standard-Design für Direct Update: einen Dialog mit einer Standardnachricht, der angezeigt wird, wenn ein Direct Update verfügbar ist, und eine Standard-Fortschrittsanzeige, die angezeigt wird, wenn der Direct Update-Prozess initiiert wird. Sie können ein angepasstes Verhalten der Direct Update-Benutzerschnittstelle implementieren oder aber den Dialog für Direct Update anpassen, indem Sie diese Funktion außer Kraft setzen und Ihre eigene Logik implementieren.

Im nachfolgenden Beispielcode implementiert eine `handleDirectUpdate`-Funktion eine angepasste Nachricht im Dialog für Direct Update. Fügen Sie diesen Code zur Datei `www/js/index.js` Ihres Cordova-Projekts hinzu.
Weitere Beispiele für eine angepasste Direct Update-Benutzerschnittstelle:
* Ein Dialog, der unter Verwendung eines JavaScript-Frameworks eines anderen Anbieters (wie z. B. Dojo oder jQuery Mobile, Ionic u. ä.) erstellt wird
* Eine vollständig native Benutzerschnittstelle durch Ausführung eines Cordova-Plug-ins
* Eine alternative HTML, die dem Benutzer mit Optionen dargestellt wird, usw.

```JavaScript
wl_directUpdateChallengeHandler.handleDirectUpdate = function(directUpdateData, directUpdateContext) {        
    navigator.notification.confirm(  // Erstellt einen Dialog.
        'Custom dialog body text',
        // Verarbeitung von Dialogschaltflächen.
          directUpdateContext.start();
        },
        'Custom dialog title text',
        ['Update']
    );
};
```
{: codeblock}

Sie können den Direct Update-Prozess starten, indem die Methode `directUpdateContext.start()` immer dann ausgeführt wird, wenn der Benutzer auf die Dialogschaltfläche klickt. Die Standard-Fortschrittsanzeige ähnlich der in Vorgängerversionen des MobileFirst-Servers wird angezeigt.

Diese Methode unterstützt die folgenden Aufruftypen:
* Sind keine Parameter angegeben, verwendet der MobileFirst-Server die Standard-Fortschrittsanzeige.
* Wenn eine Listenerfunktion wie z. B. `directUpdateContext.start(directUpdateCustomListener)` angegeben wird, wird der Direct Update-Prozess im Hintergrund ausgeführt, während der Prozess Lebenszyklusereignisse an den Listener sendet. Der angepasste Listener muss die folgenden Methoden implementieren:

```JavaScript
var  directUpdateCustomListener  = {
    onStart : function ( totalSize ){ },
    onProgress : function ( status , totalSize , completedSize ){ },
    onFinish : function ( status ){ }
};
```
{: codeblock}

Die Listenermethoden werden während des Direct Update-Prozesses nach den folgenden Regeln gestartet:
* `onStart` wird mit dem Parameter `totalSize` aufgerufen, der die Größe der Aktualisierungsdatei enthält.
* `onProgress` wird mehrere Male mit den Status `DOWNLOAD_IN_PROGRESS`, `totalSize` und `completedSize` (das bisher heruntergeladene Volumen) aufgerufen.
* `onProgress` wird mit dem Status `UNZIP_IN_PROGRESS` aufgerufen.
* `onFinish` wird mit einem der folgenden Statuscodes aufgerufen:

|Statuscode | Beschreibung |
|:------------|:------------|
| `SUCCESS` | Direct Update ohne Fehler fertiggestellt. |
| `CANCELED` | Direct Update wurde abgebrochen (z. B. weil die Methode `stop()` aufgerufen wurde). |
| `FAILURE_NETWORK_PROBLEM` | Während der Aktualisierung ist ein Problem mit der Netzverbindung aufgetreten. |
| `FAILURE_DOWNLOADING` |Die Datei wurde nicht vollständig heruntergeladen. |
| `FAILURE_NOT_ENOUGH_SPACE` | Der Platz im Gerät reicht nicht aus, um die Aktualisierungsdatei herunterzuladen und zu entpacken. |
| `FAILURE_UNZIPPING` | Beim Entpacken der Aktualisierungsdatei ist ein Problem aufgetreten. |
| `FAILURE_ALREADY_IN_PROGRESS` | Die Startmethode wurde aufgerufen, während Direct Update bereits ausgeführt wurde. |
| `FAILURE_INTEGRITY` | Die Authentizität der Aktualisierungsdatei kann nicht verifiziert werden. |
| `FAILURE_UNKNOWN` | Unerwarteter interner Fehler. |

Wenn Sie einen angepassten Listener für Direct Update implementieren, achten Sie darauf, dass die App neu geladen wird, nachdem der Direct Update-Prozess abgeschlossen ist, und die Methode `onFinish()` aufgerufen wurde. Wenn der Direct Update-Prozess nicht erfolgreich beendet wird, müssen Sie zudem `wl_directUpdateChalengeHandler.submitFailure()` aufrufen.

Das folgende Beispiel zeigt eine Implementierung eines angepassten Listeners für Direct Update:
```JavaScript
var directUpdateCustomListener = {
  onStart: function(totalSize){
    //show custom progress dialog
  },
  onProgress: function(status,totalSize,completedSize){
    //update custom progress dialog
  },
  onFinish: function(status){

    if (status == 'SUCCESS'){
      //show success message
      WL.Client.reloadApp();
    }
    else {
      //angepasste Fehlernachricht anzeigen

      //Bei einem Fehler muss submitFailure aufgerufen werden
      wl_directUpdateChallengeHandler.submitFailure();
    }
  }
};

wl_directUpdateChallengeHandler.handleDirectUpdate = function(directUpdateData, directUpdateContext){

  WL.SimpleDialog.show('Update Avalible', 'Press update button to download version 2.0', [{
    text : 'update',
    handler : function() {
      directUpdateContext.start(directUpdateCustomListener);
    }
  }]);
};
```
{: codeblock}

### Szenario: Direct Updates ohne Benutzerschnittstelle ausführen
{: #scenario-running-ui-less-direct-updates }
{{site.data.keyword.mobilefoundation_short}} unterstützt Direct Updates ohne Benutzerschnittstelle, wenn die Anwendung im Vordergrund aktiv ist.

Um Direct Updates ohne Benutzerschnittstelle auszuführen, implementieren Sie `directUpdateCustomListener`. Geben Sie leere Funktionsimplementierungen für die Methoden `onStart` und `onProgress` an. Leere Implementierungen führen dazu, dass der Direct Update-Prozess im Hintergrund ausgeführt wird.

Um den Direct Update-Prozess zu vervollständigen, muss die Anwendung erneut geladen werden. Folgende Optionen sind verfügbar:
* Die Methode `onFinish` darf ebenfalls leer sein. In diesem Fall wird Direct Update angewendet, nachdem die Anwendung gestartet wurde.
* Sie können einen angepassten Dialog implementieren, der den Benutzer informiert bzw. ihn auffordert, die Anwendung erneut zu starten. (Siehe dazu das folgende Beispiel.)
* Die Methode `onFinish` kann ein erneutes Laden der Anwendung erzwingen, indem sie `WL.Client.reloadApp()` aufruft.

Hier eine Beispielimplementierung von `directUpdateCustomListener`:

```JavaScript
var directUpdateCustomListener = {
  onStart: function(totalSize){
  },
  onProgress: function(status,totalSize,completeSize){
  },
  onFinish: function(status){
    WL.SimpleDialog.show('New Update Available', 'Press reload button to update to new version', [ {
      text : WL.ClientMessages.reload,
      handler : WL.Client.reloadApp
    }]);
  }
};
```
{: codeblock}

Implementieren Sie die Funktion `wl_directUpdateChallengeHandler.handleDirectUpdate`. Leiten Sie die `directUpdateCustomListener`-Implementierung, die Sie als Parameter erstellt haben, an die Funktion weiter. Achten Sie darauf, dass `directUpdateContext.start(directUpdateCustomListener`) aufgerufen wird. Hier eine Beispielimplementierung von `wl_directUpdateChallengeHandler.handleDirectUpdate`:

```JavaScript
wl_directUpdateChallengeHandler.handleDirectUpdate = function(directUpdateData, directUpdateContext){

  directUpdateContext.start(directUpdateCustomListener);
};
```
{: codeblock}

**Hinweis:** Wenn die Anwendung in den Hintergrund gesendet wird, wird der Direct Update-Prozess ausgesetzt.

### Szenario: Fehlschlagen eines Direct Update behandeln
{: #scenario-handling-a-direct-update-failure }
Dieses Szenario zeigt die Behandlung eines fehlgeschlagenen Direct Update, wie es z. B. durch eine Verbindungsunterbrechung verursacht werden könnte. In diesem Szenario wird der Benutzer selbst im Offlinemodus daran gehindert, die App zu verwenden. Ein Dialog wird angezeigt, der dem Benutzer die Option anbietet, den Versuch zu wiederholen.

Erstellen Sie eine globale Variable, um den Direct Update-Kontext zu speichern, sodass Sie ihn später, wenn der Direct Update-Prozess fehlschlägt, verwenden können. Beispiel:

```JavaScript
var savedDirectUpdateContext;
```
{: codeblock}

Implementieren Sie einen Abfrage-Handler für Direct Updates. Speichern Sie den Direct Update-Kontext an diesem Ort. Beispiel:

```JavaScript
wl_directUpdateChallengeHandler.handleDirectUpdate = function(directUpdateData, directUpdateContext){

  savedDirectUpdateContext = directUpdateContext; // Direct Update-Kontext speichern

  var downloadSizeInMB = (directUpdateData.downloadSize / 1048576).toFixed(1).replace(".", WL.App.getDecimalSeparator());
  var directUpdateMsg = WL.Utils.formatString(WL.ClientMessages.directUpdateNotificationMessage, downloadSizeInMB);

  WL.SimpleDialog.show(WL.ClientMessages.directUpdateNotificationTitle, directUpdateMsg, [{
    text : WL.ClientMessages.update,
    handler : function() {
      directUpdateContext.start(directUpdateCustomListener);
    }
  }]);
};
```
{: codeblock}

Erstellen Sie eine Funktion, die den Direct Update-Prozess mithilfe des Direct Update-Kontextes startet. Beispiel:

```JavaScript
restartDirectUpdate = function () {
  savedDirectUpdateContext.start(directUpdateCustomListener); // gespeicherten Direct Update-Kontext für den Neustart von Direct Update verwenden
};
```
{: codeblock}

Implementieren Sie `directUpdateCustomListener`. Fügen Sie die Statusprüfung in der Methode `onFinish` hinzu. Beginnt der Status mit `FAILURE`, so öffnen Sie einen nur modalen Dialog mit der Option **Vorgang wiederholen**. Beispiel:

```JavaScript
var directUpdateCustomListener = {
  onStart: function(totalSize){
    alert('onStart: totalSize = ' + totalSize + 'Byte');
  },
  onProgress: function(status,totalSize,completeSize){
    alert('onProgress: status = ' + status + ' completeSize = ' + completeSize + 'Byte');
  },
  onFinish: function(status){
    alert('onFinish: status = ' + status);
    var pos = status.indexOf("FAILURE");
    if (pos > -1) {
      WL.SimpleDialog.show('Update Failed', 'Press try again button', [ {
        text : "Try Again",
        handler : restartDirectUpdate // Direct Update neu starten
      }]);
    }
  }
};
```
{: codeblock}

Wenn der Benutzer auf die Schaltfläche **Vorgang wiederholen** klicke, startet die Anwendung den Direct Update-Prozess neu.

## Delta- und vollständiges Direct Update
{: #delta-and-full-direct-update }
Delta-Direct Updates ermöglichen einer Anwendung das Herunterladen nur derjenigen Dateien, die seit der letzten Aktualisierung geändert wurden, statt aller Webressourcen der Anwendung. Dies verringert die Downloadzeit, bewahrt die Bandbreite und verbessert die Benutzererfahrung insgesamt.

> <span class="glyphicon glyphicon-exclamation-sign" aria-hidden="true"></span> **Wichtig:** Ein **Delta-Update** ist nur dann möglich, wenn die Webressourcen der Clientanwendung um genau eine Version hinter der aktuell auf dem Server implementierten liegen. Clientanwendungen, die mehr als eine Version hinter der aktuell implementierten Anwendung liegen (d. h. die Anwendung wurde seit der Aktualisierung der Clientanwendung mindestens zweimal auf dem Server implementiert), erhalten eine **vollständige Aktualisierung** (d. h. sämtliche Webressourcen werden heruntergeladen und aktualisiert).


## Beispielanwendung
{: #sample-application }
[Klicken Sie, um das ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/MobileFirst-Platform-Developer-Center/CustomDirectUpdate/tree/release80) Cordova-Projekt herunterzuladen.  

### Verwendungsbeispiel
{: #sample-usage }
Folgen Sie den Anweisungen in der README.md-Datei des Beispiels.

## CDN-Unterstützung
{: #cdn_support}

Sie können Anforderungen für Direct Update so konfigurieren, dass sie über ein CDN (content delivery network, Netz zur Bereitstellung von Inhalten) statt über den MobileFirst-Server bedient werden.

### Vorteile der Verwendung eines CDN
{: #advantages-of-using-a-cdn }
Ein CDN anstelle des MobileFirst-Servers für die Bedienung von Direct Update-Anforderungen zu verwenden, hat folgende Vorteile:

* Netzwerkaufwände werden vom MobileFirst-Server entfernt.
* Übertragungsraten über dem Grenzwert von 250 MB/Sekunden beim Bedienen von Anforderungen von Seiten eines MobileFirst-Servers werden erhöht.
* Eine einheitlichere Erfahrung der Benutzer eines Direct Update unabhängig von ihrem Standort wird sichergestellt.

### Allgemeine Voraussetzungen
{: #general-requirements }
Damit Direct Update-Anforderungen über ein CDN bedient werden können, stellen Sie sicher, dass Ihre Konfiguration den folgenden Bedingungen entspricht:

* Das CDN muss ein Reverse Proxy vor dem MobileFirst-Server (oder bei Bedarf vor einem anderen Reverse Proxy) sein.
* Beim Erstellen der Anwendung über Ihre Entwicklungsumgebung richten Sie Ihren Zielserver mit dem CDN-Host und -Port ein statt mit dem Host und Port des MobileFirst-Servers. Wenn Sie z. B. den MobileFirst-CLI-Befehl `mfpdev server add` ausführen, geben Sie den CDN-Host und -Port an.
* In der CDN-Verwaltungsanzeige müssen Sie die folgenden Direct Update-URLs für das Caching markieren, um sicherzustellen, dass das CDN alle Anforderungen an den MobileFirst-Server außer den Direct Update-Anforderungen weiterleitet. Bei Direct Update-Anforderungen entscheidet das CDN, ob der Inhalt abgerufen wurde. Ist das der Fall, gibt ihn das CDN zurück, ohne den MobileFirst-Server aufzurufen. Ist es nicht der Fall, ruft es den MobileFirst-Server auf, ruft das Direct Update-Archiv (ZIP-Datei) auf und speichert diese für die nächsten Anforderungen nach dieser einen URL. Bei Anwendungen mit Version 8.0 von {{site.data.keyword.mobilefoundation_short}} lautet die Direct Update-URL: `PROTOCOL://DOMAIN:PORT/CONTEXT_PATH/api/directupdate/VERSION/CHECKSUM/TYPE`.
Das Präfix `PROTOCOL://DOMAIN:PORT/CONTEXT_PATH` ist bei allen Laufzeitanforderungen konstant. Beispiel: `http://my.cdn.com:9080/mfp/api/directupdate/0.0.1/742914155/full?appId=com.ibm.DirectUpdateTestApp&clientPlatform=android`

In diesem Beispiel gibt es weitere Anforderungsparameter, die ebenfalls Teil der Anforderung sind.

* Das CDN muss das Caching der Anforderungsparameter zulassen. Zwei unterschiedliche Direct Update-Archive dürfen sich nur anhand der Anforderungsparameter unterscheiden.
* Das CDN muss TTL in der Antwort von Direct Update unterstützen. Die Unterstützung wird bei mehreren Direct Updates für dieselbe Version benötigt.
* Das CDN darf die im Server-Client-Protokoll verwendeten HTTP-Header nicht ändern oder entfernen.

### Beispiel für eine CDN-Konfiguration
{: #example-cdn-configuration }
Dieses Beispiel basiert auf der Verwendung einer Akamai-CDN-Konfiguration, die das Direct Update-Archiv zwischenspeichert. Die folgenden Tasks werden vom Netzadministrator, dem MobileFirst-Administrator und dem Akamai-Administrator ausgeführt:

#### Netzadministrator
{: #network-administrator }
Erstellen Sie eine andere Domäne im DNS für Ihren MobileFirst-Server. Wenn Ihre Serverdomäne z. B. `yourcompany.com` ist, müssen Sie eine zusätzliche Domäne wie z. B. `cdn.yourcompany.com` erstellen.
Legen Sie im DNS für die neue Domäne `cdn.yourcompany.com` einen `CNAME` für den Domänennamen fest, der von Akamai bereitgestellt wird. Beispiel: `yourcompany.com.akamai.net`.

#### MobileFirst-Administrator
{: #mobilefirst-administrator }
Legen Sie die neue Domäne `cdn.yourcompany.com` als MobileFirst-Server-URL für die MobileFirst-Anwendung fest. Für die Ant-Builder-Task beispielsweise ist die Eigenschaft:
```xml
<property name="wl.server" value="http://cdn.yourcompany.com/${contextPath}/"/>
```
{: codeblock}

#### Akamai-Administrator
{: #akamai-administrator }
1. Öffnen Sie den Akamai-Eigenschaftenmanager und legen Sie die Eigenschaft **host name** auf den Wert der neuen Domäne fest.

    ![Legen Sie den Hostnamen der Eigenschaft auf den Wert der neuen Domäne fest](images/direct_update_cdn_3.jpg)

2. Auf der Registerkarte für Standardregeln konfigurieren Sie den ursprünglichen Host und Port des MobileFirst-Servers und legen den Wert für **Benutzerdefinierten Host-Header weiterleiten** auf die neu erstellte Domäne fest.

    ![Legen Sie den Wert für 'Benutzerdefinierten Host-Header weiterleiten' auf den Wert der neu erstellten Domäne fest](images/direct_update_cdn_4.jpg)

3. Wählen Sie aus der Liste **Caching-Option** den Eintrag **Kein Speicher**.

    ![Wählen Sie aus der Liste 'Caching-Option' 'Kein Speicher' aus](images/direct_update_cdn_5.jpg)

4. Konfigurieren Sie auf der Registerkarte **Konfiguration von statischem Inhalt** die Übereinstimmungskriterien entsprechend der Direct Update-URL der Anwendung. Erstellen Sie z. B. eine Bedingung, die aussagt: `If Path matches one of direct_update_URL` (Wenn Pfad mit einer Direct Update-URL übereinstimmt).

    ![Konfigurieren Sie die Übereinstimmungskriterien entsprechend der Direct Update-URL der Anwendung](images/direct_update_cdn_6.jpg)

5. Konfigurieren Sie das Cacheschlüsselverhalten für die Verwendung aller Anforderungsparameter im Cacheschlüssel (dies ist erforderlich, um verschiedene Direct Update-Archive für unterschiedliche Anwendungen oder Versionen zwischenzuspeichern). Wählen Sie beispielsweise aus der Liste **Verhalten** den Eintrag `Include all parameters (preserve order from request)` (Alle Parameter einschließen (Reihenfolge der Anforderung beibehalten)) aus.

    ![Konfigurieren Sie das Cacheschlüsselverhalten für die Verwendung aller Anforderungsparameter im Cacheschlüssel](images/direct_update_cdn_8.jpg)

6. Legen Sie Werte ähnlich den folgenden Werten fest, um das Caching-Verhalten so zu konfigurieren, dass die Direct Update-URL zwischengespeichert wird, und um TTL festzulegen.

      ![Legen Sie Werte für zur Konfiguration des Caching-Verhaltens fest](images/direct_update_cdn_7.jpg)

| Feld | Wert |
|:------|:------|
| Caching-Option | Cache |
| Neubewertung veralteter Objekt erzwingen | Veraltete Objekte bedienen, wenn sie nicht validiert werden können |
| Höchstalter | 3 Minuten |


## Secure Direct Update
{: #secure-dc }

Secure Direct Update, das standardmäßig inaktiviert ist, hindert Angreifer von außen daran, die Webressourcen zu ändern, die vom MobileFirst-Server (oder von einem CDN (Content Delivery Network)) an die Clientanwendung übertragen werden.

**Um die Direct Update-Authentizität zu aktivieren, gehen Sie folgendermaßen vor:**  
Extrahieren Sie mit Ihrem bevorzugten Werkzeug den öffentlichen Schlüssel vom MobileFirst-Server-Keystore und konvertieren Sie ihn nach Base64.  
Der erzeugte Wert sollte anschließend wie unten beschrieben verwendet werden:

1. Öffnen Sie ein **Befehlszeilenfenster** und navigieren Sie zum Stamm des Cordova-Projekts.
2. Führen Sie den Befehl `mfpdev app config` aus und wählen Sie die Option **Öffentlicher Schlüssel für Direct Update-Authentizität** aus.
3. Geben Sie den öffentlichen Schlüssel an und bestätigen Sie die Eingabe.

Alle künftigen Direct Update-Bereitstellungen an Clientanwendungen werden durch die Direct Update-Authentizität geschützt.

Damit Secure Direct Update funktioniert, muss eine benutzerdefinierte Keystore-Datei auf dem MobileFirst-Server implementiert und eine Kopie des übereinstimmenden öffentlichen Schlüssels in die implementierte Clientanwendung eingeschlossen werden.

In diesem Thema wird das Binden eines öffentlichen Schlüssels an neue Clientanwendungen und vorhandene Clientanwendungen nach einem Upgrade beschrieben. Weitere Informationen zum Konfigurieren des Keystores im MobileFirst-Server finden Sie unter [MobileFirst-Server-Keystore konfigurieren ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/configuring-the-mobilefirst-server-keystore/){: new_window}

Der Server stellt einen integrierten Keystore bereit, der zum Testen von Secure Direct Update in Entwicklungsphasen verwendet werden kann.

>**Hinweis:** Nach dem Binden des öffentlichen Schlüssels an die Clientanwendung und nach der Neuerstellung müssen Sie sie nicht nochmals auf {{ site.data.keys.mf_server }} hochladen. Wenn Sie jedoch zuvor die Anwendung ohne öffentlichen Schlüssel im Markt veröffentlicht haben, muss sie erneut veröffentlicht werden.

Für Entwicklungszwecke wird der folgende Standard-Dummy eines öffentlichen Schlüssels mit dem MobileFirst-Server bereitgestellt:

```xml
-----BEGIN PUBLIC KEY-----
MIIDPjCCAiagAwIBAgIEUD3/bjANBgkqhkiG9w0BAQsFADBgMQswCQYDVQQGEwJJTDELMAkGA1UECBMCSUwxETA
PBgNVBAcTCFNoZWZheWltMQwwCgYDVQQKEwNJQk0xEjAQBgNVBAsTCVdvcmtsaWdodDEPMA0GA1UEAxMGV0wgRG
V2MCAXDTEyMDgyOTExMzkyNloYDzQ3NTAwNzI3MTEzOTI2WjBgMQswCQYDVQQGEwJJTDELMAkGA1UECBMCSUwxE
TAPBgNVBAcTCFNoZWZheWltMQwwCgYDVQQKEwNJQk0xEjAQBgNVBAsTCVdvcmtsaWdodDEPMA0GA1UEAxMGV0wg
RGV2MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAzQN3vEB2/of7KAvuvyoIt0T7cjaSTjnOBm0N3+q
zx++dh92KpNJXj/a3o4YbwJXkJ7jU8ykjCYvjXRf0hme+HGhiIVwxJo54iqh76skDS5m7DaseFdndZUJ4p7NFVw
I5ixA36ZArSZ/Pn/ej56/RRjBeRI7AEGXUSGojBUPA6J6DYkwaXQRew9l+Q1kj4dTigyKL5Os0vNFaQyYu+bT2E
vnOixQ0DXm94IqmHZamZKbZLrWcOEfuAsSjKYOdMSM9jkCiHaKcj7fpEZhUxRRs7joKs1Ri4ihs6JeUvMEiG4gK
l9V3FP/Huy0pfkL0F8xMHgaQ4c/lxS/s3PV0OEg+7wIDAQABMA0GCSqGSIb3DQEBCwUAA4IBAQAgEhhqRl2Rgkt
MJeqOCRcT3uyr4XDK3hmuhEaE0nOvLHi61PoLKnDUNryWUicK/W+tUP9jkN5xRckdzG6TJ/HPySmZ7Adr6QRFu+
xcIMY+/S8j4PHLXBjoqgtUMhkt7S2/thN/VA6mwZpw4Ol0Pa2hyT2TkhQoYYkRwYCk9pxmuBCoH/eCWpSxquNny
RwrY25x0YzccXUaMI8L3/3hzq3mW40YIMiEdpiD5HqjUDpzN1funHNQdsxEIMYsWmGAwOdV5slFzyrH+ErUYUFA
pdGIdLtkrhzbqHFwXE0v3dt+lnLf21wRPIqYHaEu+EB/A4dLO6hm+IjBeu/No7H7TBFm
-----END PUBLIC KEY-----
```
{: codeblock}

** Wichtig:** Den öffentlichen Schlüssel nicht für Produktionszwecke verwenden.

### Keystore generieren und implementieren
{: #generating-and-deploying-the-keystore }
Zum Generieren von Zertifikaten und Extrahieren von öffentlichen Schlüsseln aus einem Keystore stehen zahlreiche Tools zur Verfügung. Das folgende Beispiel veranschaulicht die Prozeduren mit dem JDK-Dienstprogramm 'keytool' und OpenSSL.

1. Extrahieren Sie den öffentlichen Schlüssel aus der Keystore-Datei, die auf dem {{ site.data.keys.mf_server }} implementiert ist.  
   >**Hinweis:** Der öffentliche Schlüssel muss Base64-codiert sein.

   Beispiel: Der Aliasname ist `mfp-server` und die Keystore-Datei ist **keystore.jks**.  
   Um ein Zertifikat zu generieren, geben Sie folgenden Befehl aus:

   ```bash
   keytool -export -alias mfp-server -file certfile.cert
   -keystore keystore.jks -storepass keypassword
   ```
   {: codeblock}

   Eine Zertifikatsdatei wird generiert.  
   Geben Sie folgenden Befehl aus, um den öffentlichen Schlüssel zu extrahieren:

   ```bash
   openssl x509 -inform der -in certfile.cert -pubkey -noout
   ```
   {: codeblock}

   > **Hinweis:** Keytool allein kann keine öffentlichen Schlüssel im Base64-Format extrahieren.

2. Führen Sie eine der folgenden Prozeduren aus:
    * Kopieren Sie den Ergebnistext ohne die Markierungen `BEGIN PUBLIC KEY` und `END PUBLIC KEY` in die Eigenschaftendatei 'mfpclient' der Anwendung direkt nach `wlSecureDirectUpdatePublicKey`.
    * Geben Sie über die Eingabeaufforderung den folgenden Befehl aus: `mfpdev app config direct_update_authenticity_public_key <public_key>`

    Für `<public_key>` fügen Sie den resultierenden Text aus Schritt 1 ohne die Markierungen `BEGIN PUBLIC KEY` und `END PUBLIC KEY` ein.

3. Führen Sie den Cordova-Buildbefehl aus, um den öffentlichen Schlüssel in der Anwendung zu speichern.
