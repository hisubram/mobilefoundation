---

copyright:
  years: 2018, 2019
lastupdated: "2018-11-22"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Informationen in der Konsole visualisieren
{: #visualize_insights_on_console}

In der MobileFirst Analytics Console können Sie Analyseberichte anzeigen und konfigurieren, Alerts verwalten und Clientprotokolle anzeigen. Starten Sie die Analytics Console über die Mobile Foundation Operations Console durch Klicken auf **Analytics Console** im linken Navigationsbereich.

Die Analytics Console wird gestartet und das Standarddashboard wird angezeigt. Wenn eine Clientanwendung bereits Protokolle und Analysedaten an den Server gesendet hat, werden die relevanten Berichte erstellt und angezeigt. Über das Dashboard können Sie die erfassten Analysedaten überprüfen. Diese Analysedaten können sich auf Anwendungsabstürze, Anwendungssitzungen und die Serververarbeitungszeit beziehen. Darüber hinaus können Sie angepasste Diagramme erstellen und Alerts verwalten.

Neben einer Übersicht über Ihre mobile Analyse bietet die Analysefunktion die Möglichkeit, eine unformatierte Suche in Clientprotokollen, erfassten Clientabsturzdaten und allen zusätzlichen Daten durchzuführen, die Sie explizit über Client-API-Funktionsaufrufe bereitstellen, die Daten an Mobile Analytics übergeben.

## App-Daten überwachen
{: #monitoring_app_data}

Mobile Analytics stellt Überwachung und Analyse für Ihre mobilen Anwendungen bereit. Mit dem Mobile Analytics-Client-SDK können Sie Anwendungsprotokolle aufzeichnen und Daten überwachen. Entwickler können steuern, wann diese Daten an den Mobile Analytics-Service gesendet werden sollen. Wenn die Daten an Mobile Analytics übermittelt werden, können Sie über die Mobile Analytics Console Analyseinformationen zu Ihren mobilen Anwendungen, Geräten und Anwendungsprotokollen erhalten.

Nachstehend einige Beispiele für verfügbare Informationen:

**Vordefinierte Messgrößen**

Mit vordefinierten Metriken können Sie beispielsweise folgende Fragen beantworten:
* Wie viele neue Benutzer habe ich?
* Wie viele Personen verwenden meine Anwendung aktiv?
* Wie häufig verwenden Personen meine Anwendung?
* Zu welcher Tageszeit verwenden Personen meine Anwendung?
* Welche Gerätemodelle bevorzugen meine Benutzer?
* Wann sollte ich die Unterstützung für traditionelle Betriebssysteme einstellen?
* Welche Anwendungen haben Leistungsprobleme?

**Angepasste Ereignisse**

Wenn Sie eigene angepasste Ereignisse hinzufügen, können Sie beispielsweise folgende Fragen beantworten:
* Welche Funktionen werden am meisten bzw. am wenigsten genutzt?
* Wo betreten bzw. verlassen Benutzer meine App?
* Welche Aktivitäten sehen sich Benutzer am meisten an?
* Führen die Benutzer Workflows in der App aus (z. B. Konvertierungstrichter)?

## Berichte, die visualisiert werden können: Benutzer
{: #reports_visualized_users}

Dieser Bericht zeigt ein Diagramm mit der Anzahl der aktiven Benutzer an, die die App in einem angegebenen Datumsbereich verwendet haben. In dem Bericht wird auch die Anzahl neuer, eindeutiger Benutzer angezeigt, die die App in dem angegebenen Datumsbereich zum ersten Mal verwenden.
Die Diagramme können nach Anwendungsname, Betriebssystem oder Betriebssystemversion gefiltert werden.

## Berichte, die visualisiert werden können: Sitzungen
{: #reports_visualized_sessions}

Dieser Bericht zeigt ein Diagramm mit den App-Sitzungen in einem angegebenen Datumsbereich an. Eine Sitzung wird aufgezeichnet, wenn eine App in den Vordergrund eines Geräts gebracht wird. Die Diagramme können nach Anwendungsname, Betriebssystem oder Betriebssystemversion gefiltert werden.

## Berichte, die visualisiert werden können: Netzanforderungen
{: #reports_visualized_network_requests}

Sie können die Netzanforderungsdaten Ihrer Anwendungen in der Mobile Analytics Console visualisieren.

Für die folgenden Messungen sind Daten verfügbar:

**Umlaufzeit** - Definiert die Zeitdauer (gemessen in Millisekunden), die Ihre App benötigt, um Netzanforderungen abzusetzen.
**Anzahl der Anforderungen** - Zeigt an, wie oft eine App Netzanforderungen absetzt. Die Daten werden auch als Durchschnittswert angezeigt.
Die Diagramme können nach Anwendungsname, Betriebssystem oder Betriebssystemversion gefiltert werden.

## Berichte, die visualisiert werden können: Abstürze
{: #reports_visualized_crashes}

Sie können in der Mobile Analytics Console Informationen zu Ihrer Anwendung anzeigen, um die Überwachung und Fehlerbehebung Ihrer Anwendungen zu verbessern.

Auf der Seite "Abstürze" werden in der Tabelle **Übersicht über Abstürze** die folgenden Datenspalten angezeigt:

**Anwendung**: Anwendungsname<br/>
**Abstürze**: Gesamtzahl der Abstürze dieser App<br/>
**Gesamtnutzung**: Gesamtzahl der Vorgänge, bei denen ein Benutzer diese App öffnet und schließt<br/>
**Absturzrate**: Prozentsatz der Abstürze pro Nutzung<br/>
In der Tabelle "Abstürze" können Sie schnell Informationen zu den Abstürzen Ihrer Anwendung anzeigen. Das Balkendiagramm "Abstürze" zeigt ein Histogramm der Abstürze in einem bestimmten Zeitraum an.<br/>

Sie können Absturzdaten auf zwei Arten anzeigen:

1.  Absturzrate anzeigen: Absturzrate in einem bestimmten Zeitraum
2.  Gesamtzahl der Abstürze anzeigen: Gesamtzahl der Abstürze in einem bestimmten Zeitraum

## Berichte, die visualisiert werden können: Fehlerbehebung
{: #reports_visualized_troubleshooting}

Die Seite **Fehlerbehebung** in der Mobile Analytics Console bietet in der Tabelle **Absturzzusammenfassung** eine differenzierte Anzeige Ihrer App-Abstürze.

Die Tabelle **Absturzzusammenfassung** lässt sich sortieren. Sie enthält die folgenden Datenspalten:

* Abstürze
* Geräte
* Letzter Absturz
* Anwendung
* Betriebssystem
* Nachricht

Klicken Sie neben einem der Einträge auf das Symbol "+", um die Tabelle **Absturzdetails** anzuzeigen, die die folgenden Spalten enthält:

* Absturzzeit
* Anwendungsversion
* Betriebssystemversion
* Gerätemodell
* Geräte-ID
* Download: Link zum Herunterladen der Protokolle, die zum Absturz geführt haben

Erweitern Sie einen beliebigen Eintrag in der Tabelle **Absturzdetails**, um weitere Details wie einen Stack-Trace zu erhalten.

Die Tabelle **Absturzzusammenfassung** wird mit Daten aus Abfragen der Anwendungsprotokolle mit schwerwiegenden Fehlern gefüllt. Wenn Ihre Anwendung keine schwerwiegenden Anwendungsprotokolle erfasst, sind keine Daten verfügbar.
{: note}


## Berichte, die visualisiert werden können: Benutzerfeedback
{: #reports_visualized_userfeedback}

Das Benutzerfeedback ermöglicht eine Feedbackanalyse in einer App mithilfe von Mobile Analytics.
Diese Funktion von Mobile Analytics bietet folgende Vorteile:
* **Benutzer und Tester** können Feedback und Programmfehlerberichte zu einer Anwendung aufzeichnen und senden, während sie diese ausführen und verwenden.
* **Anwendungseigner** erhalten mit diesem kontextreichen Benutzerfeedback einen tieferen Einblick in das Benutzererlebnis bezüglich der Anwendung.
* **Entwickler** dagegen erhalten präzise Anwendungskontexte, um Programmfehler oder Funktionslücken zu diagnostizieren und zu beheben.

### Benutzerfeedback ermöglichen
{: #enable_user_feedback}

Führen Sie die folgenden Schritte aus, um die Erfassung von Benutzerfeedback durch Ihre mobile Anwendung zu ermöglichen.

#### App instrumentieren
{: #instrument_app}

* Instrumentieren Sie Ihre mobile App so, dass sie in den Feedbackmodus wechselt. Rufen Sie die API `Analytics.triggerFeedbackMode();` auf, um den Feedbackmodus aufzurufen. <!--For more information, refer to the documentation [here](instrument_an_app.html)-->.
* Die API kann bei jedem Anwendungsereignis aufgerufen werden, beispielsweise bei Schaltflächen, Menüaktionen oder Gesten.

#### Benutzerfeedback erhalten
{: #receive_feedback}

* Benutzer und Tester Ihrer App können in den Feedbackmodus umschalten, indem Sie die Anwendungsaktion auslösen, die für Feedback instrumentiert ist.
* Im Feedbackmodus können kontextreiches Feedback und ein Screenshot erfasst und an Mobile Analytics gesendet werden.

#### Benutzerfeedback analysieren
{: #analyze_feedback}

* Mobile Analytics empfängt und konsolidiert das von den mobilen Anwendungen gesendete kontextreiche Feedback.
* Melden Sie sich an der Mobile Analytics Console an und wählen Sie die Option **Benutzerfeedback** aus, um das Feedback anzuzeigen.
* Ein Anwendungseigner kann das Feedback prüfen, Kommentare hinzufügen und das Feedback mit einem Prüfstatus markieren. Typische Kommentare können geplante Aktionen wie Links zu Git-Problemen sein, die für die Arbeit mit dem Feedback erstellt werden, oder aber Aussagen, die begründen, warum für das Feedback keine Aktion erforderlich ist.
* Der Prüfstatus kann verwendet werden, um Feedback effizient zu verwalten, indem Sie es in einer der verschiedenen Prüfstatusoptionen kategorisieren.

