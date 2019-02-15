---

copyright:
  years: 2018, 2019
lastupdated: "2018-10-16"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}

# Ressourcen verschleiern
{: #obfuscate_resources}

Bei einer Verschleierung wird eine ausführbare Funktion so geändert, dass sie für einen Hacker nicht mehr nützlich ist, jedoch voll funktionsfähig bleibt. Die Verschleierung von automatisiertem Code macht die Rückentwicklung eines Programms schwierig. 

Durch eine Verschleierung kann eine Anwendung nur schwer zurückentwickelt werden und ist vor dem Diebstahl von Geschäftsgeheimnissen (geistiges Eigentum), unbefugtem Zugriff, der Umgehung der Lizenzierung und anderer Kontrollmechanismen sowie vor Sicherheitslücken geschützt.

Bei der Codeverschleierung kommen viele verschiedene Techniken und Anwendungssicherheitsverfahren zum Einsatz:

* Verschleierung durch Umbenennung - Durch das Umbenennen wird der Name von Methoden und Variablen geändert, was dazu führt, dass die dekompilierte Quelle schwerer zu verstehen ist, gleichzeitig jedoch die Anwendungsausführung nicht geändert wird. Diese Methode ist die bevorzugte Verschleierungsmethode von .NET-, iOS-, Java- und Android-Verschleierungsfunktionen (Obfuskatoren). 
* Zeichenfolgenverschlüsselung
* Verschleierung von Steuerungsabläufen
* Transformation von Anweisungsmustern
* Einfügung von Dummy-Code
* Schutz vor unbefugten Eingriffe (Anti-Tampering) usw.

Durch eine Verschleierung wird es für nicht berechtigte Personen oder Angreifer weitaus schwieriger, den Code zu überprüfen und die Anwendung zu analysieren. Für die Verschleierung stehen verschiedene Tools von Drittanbietern zur Verfügung.

* Informationen zur Verschleierung von Android-Anwendungen finden Sie in diesem [Blog](https://mobilefirstplatform.ibmcloud.com/blog/2016/09/19/mfp-80-obfuscating-android-code-with-proguard/).
    >**Hinweis**: Sie müssen eine Konfigurationsdatei (proguard-project.txt) für die Verschleierung von Android-Code mit ProGuard für eine MobileFirst-Android-Anwendung erstellen.

* Informationen zur Basisverschleierung von Cordova-Anwendungen finden Sie im Abschnitt zum [Verschlüsseln von Cordova-Anwendungen](#encryptingcordovapackage).

## Webressourcen der Cordova-Pakete verschlüsseln
{: #encryptingcordovapackage}

Um das Risiko zu verringern, dass jemand Ihre Webressourcen anzeigt und ändert, während diese sich im ``.apk``- oder ``.ipa``-Paket befinden, können Sie mit dem MobileFirst-Befehlszeilenschnittstellenbefehl 'mfpdev app webencrypt' oder dem Flag 'mfpwebencrypt' die Informationen verschlüsseln. Dieses Verfahren bietet zwar keine optimale Verschleierung, jedoch ein Mindestmaß an Schutz.

**Voraussetzungen**:

* Sie müssen die Cordova-Entwicklungstools installiert haben. In diesem Beispiel wird die CLI von Apache Cordova verwendet. Wenn Sie andere Cordova-Entwicklungstools verwenden, werden einige der Schritte anders sein. Entsprechende Anweisungen finden Sie in der Dokumentation zu Ihrem Cordova-Tool.
* Sie müssen die MobileFirst-CLI installiert haben.
* Sie müssen das MobileFirst-Cordova-Plug-in installiert haben.

Der geeignete Zeitpunkt zum Durchführen dieser Prozedur ist nach dem Abschluss der App-Entwicklung und vor der Bereitstellung der App. Wenn Sie einen der folgenden Befehle ausführen, nachdem Sie das Verschlüsselungsverfahren für Webressourcen abgeschlossen haben, wird der verschlüsselte Inhalt entschlüsselt:

* cordova prepare
* cordova build
* cordova run
* cordova emulate
* mfpdev app webupdate
* mfpdev app preview

Wenn Sie einen der aufgeführten Befehle ausführen, nachdem Sie die Webressourcen verschlüsselt haben, müssen Sie diese Prozedur erneut ausführen, um die Webressourcen zu verschlüsseln.

1. Öffnen Sie ein Terminalfenster und navigieren Sie zum Stammverzeichnis der Cordova-App, die Sie verschlüsseln möchten.
2. Bereiten Sie die App vor, indem Sie einen der folgenden Befehle eingeben:
    * ``cordova prepare``
    * ``mfpdev app webupdate``
3. Führen Sie eine der folgenden Prozeduren aus, um den Inhalt zu verschlüsseln:
    * Geben Sie den folgenden Befehl ein: ``mfpdev app webencrypt``.
        >**Tipp**: Sie können Informationen zu dem Befehl ``mfpdev app webencrypt`` anzeigen, indem Sie ``mfpdev help app webencrypt`` eingeben.
    * Sie können die Webressourcen Ihrer Cordova-Pakete auch verschlüsseln, indem Sie das Flag ``mfpwebencrypt`` zum Befehl ``cordova compile`` oder ``cordova build`` hinzufügen, wenn Sie die Pakete erstellen.
       * ``cordova compile -- --mfpwebencrypt`` | ``cordova build -- --mfpwebencrypt``
            Die Betriebssysteminformationen im Ordner **www** werden durch die Datei **resources.zip** ersetzt, die den verschlüsselten Inhalt enthält.
            Wenn die App für das Android-Betriebssystem konzipiert und die Datei **resources.zip** größer als 1 MB ist, wird die Datei **resources.zip** in kleinere, 768 KB große ZIP-Dateien aufgeteilt, die den Namen **resources.zip.nnn** aufweisen. Die Variable 'nnn' ist eine Zahl zwischen 001 und 999.
4. Testen Sie die Anwendung mit den verschlüsselten Ressourcen, indem Sie den Emulator verwenden, der mit den plattformspezifischen Tools bereitgestellt wird. Sie können beispielsweise den Emulator in Android Studio für Android oder Xcode für iOS verwenden.

>**Hinweis**: Verwenden Sie nicht die folgenden Cordova-Befehle, um die Anwendung zu testen, nachdem Sie sie verschlüsselt haben:

>* ``cordova run``
>* ``cordova emulate``

>Diese Befehle aktualisieren den Inhalt, der im 'www'-Ordner verschlüsselt wurde, und speichern ihn erneut als entschlüsselten Inhalt. Wenn Sie diese Befehle verwenden, müssen Sie die Prozedur erneut ausführen, um die App zu verschlüsseln, bevor Sie sie veröffentlichen.
