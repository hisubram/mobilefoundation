---

copyright:
  years: 2018
lastupdated: "2018-12-21"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:java: .ph data-hd-programlang='java'}
{:ruby: .ph data-hd-programlang='ruby'}
{:c#: .ph data-hd-programlang='c#'}
{:objectc: .ph data-hd-programlang='Objective C'}
{:python: .ph data-hd-programlang='python'}
{:javascript: .ph data-hd-programlang='javascript'}
{:php: .ph data-hd-programlang='PHP'}
{:swift: .ph data-hd-programlang='swift'}

# APIs für Cordova-/Webanwendungen (Javascript)

In der folgenden Tabelle sind die Funktionen aufgeführt, die Sie in Javascript-Anwendungen ausführen können, sowie die entsprechende API-Methode.

| Funktion | Beschreibung |
|----------|-------------|
| `WL.Client`, `WL.App` | Anwendungen initialisieren und erneut laden, Anwendungstexte globalisieren |
| `WLAuthorizationManager` | Client-ID und Berechtigungsheader abrufen |
| `WL.Logger` | Protokollnachrichten in das Protokoll für die Umgebung drucken |
| `WL.NativePage` | Von der aktuell angezeigten, webbasierten Anzeige zu einer nativ geschriebenen Seite wechseln |
| `WLResourceRequest` | Anforderungen an geschützte und ungeschützte Ressourcen senden |
| `WL.JSONStore` | Clientseitige API zur Bereitstellung eines schlanken, dokumentorientierten Speichersystems |

## Zusätzliche Informationen
{: #additional-information }
### Objekt "options"
{: #the-optional-object }
Das Objekt `options` enthält Eigenschaften, die für alle Methoden gelten. Es wird in allen asynchronen Aufrufen des {{ site.data.keys.mf_server }}s verwendet. 

Manchmal wird es durch Eigenschaften erweitert, die nur für bestimmte Methoden gelten. Diese zusätzlichen Eigenschaften werden in der Beschreibung der jeweiligen Methoden detailliert beschrieben. 

Die gemeinsamen Eigenschaften das Objekts "options" lauten wie folgt: 

```javascript
options = {
    onSuccess: successHandler(response),
    onFailure: failureHandlder(response),
    invocationContext: invocation-context
};
```
{: code}
Die einzelnen Eigenschaften haben folgende Bedeutung: 

| Eigenschaft | Beschreibung |
|----------|-------------|
| `onSuccess` | Optional. Die Funktion, die beim erfolgreichen Abschluss des asynchronen Aufrufs aufgerufen werden soll. Die Funktion `onSuccess` hat folgende Syntax: `success-handler-function(response)`. Dabei ist `response` ein Objekt, das mindestens die folgende Eigenschaft enthält: {::nomarkdown}<ul><li><b>invocationContext</b> - Das Objekt <code>invocationContext</code>, das ursprünglich an den {{site.data.keys.mf_server }} im Objekt <code>options</code> übergeben wurde, oder <code>undefined</code>, wenn kein Objekt <code>invocationContext</code> übergeben wurde.</li><li><b>status</b> - Der Status der HTTP-Antwort</li></ul>{:/} **Hinweis:** Bei Methoden, deren Objekt `response` zusätzliche Eigenschaften enthält, werden diese Eigenschaften in der Beschreibung der jeweiligen Methoden detailliert beschrieben. |
| `onFailure` | Optional. Die Funktion, die aufgerufen werden soll, wenn der asynchrone Aufruf fehlschlägt. Dies umfasst sowohl serverseitige Fehler als auch clientseitige Fehler, die bei asynchronen Aufrufen auftreten, beispielsweise Serververbindungsfehler oder Aufrufe, bei denen das zulässige Zeitlimit überschritten wurde. **Hinweis:** Die Funktion wird nicht bei clientseitigen Fehlern aufgerufen, die eine Ausnahmebedingung auslösen und so die Ausführung stoppen. Die Syntax der Funktion "onFailure" lautet: `failure-handler-function(response)`. Dabei ist `response` ein Objekt, das die folgenden Eigenschaften enthält: {::nomarkdown}<ul><li><b>invocationContext</b> - Das Objekt <code>invocationContext</code>, das ursprünglich an den {{site.data.keys.mf_server }} im Objekt <code>options</code> übergeben wurde, oder <code>undefined</code>, wenn kein Objekt <code>invocationContext</code> übergeben wurde.</li><li><b>errorCode</b> - Eine Fehlercodezeichenfolge. Alle Fehlercodes, die zurückgegeben werden können, werden im Objekt <code>WL.ErrorCode</code> in der Datei <b>worklight.js</b> als Konstanten definiert. </li><li><b>errorMsg</b> - Eine Fehlernachricht, die vom {{site.data.keys.mf_server }} bereitgestellt wird. Diese Nachricht ist nur für den Entwickler vorgesehen und sollte dem Benutzer nicht angezeigt werden. Sie wird nicht in die Sprache des Benutzers übersetzt.</li><li><b>status</b> - Der Status der HTTP-Antwort</li></ul>{:/} **Hinweis:** Bei Methoden, deren Objekt `response` zusätzliche Eigenschaften enthält, werden diese Eigenschaften in der Beschreibung der jeweiligen Methoden detailliert beschrieben. |
| `invocationContext` | Optional. Ein Objekt, das an die Erfolgs- und Fehler-Handler zurückgegeben wird. Das Objekt `invocationContext` wird verwendet, um den Kontext des aufrufenden asynchronen Service bei der Rückgabe vom Service beizubehalten. Die Methode `invokeProcedure` kann beispielsweise mehrmals hintereinander mit demselben Erfolgs-Handler aufgerufen werden. Der Erfolgs-Handler muss in der Lage sein, denjenigen Aufruf von "invokeProcedure" zu ermitteln, der gerade ausgeführt wird. Eine Lösung besteht darin, das Objekt `invocationContext` als Ganzzahl zu implementieren und ihren Wert bei jedem Aufruf von `invokeProcedure` um 1 zu erhöhen. Beim Aufruf des Erfolgs-Handlers übergibt das {{ site.data.keys.product_adj }}-Framework ihm das Objekt `invocationContext` des Objekts "options", das der Methode `invokeProcedure` zugeordnet ist. Mit dem Wert des Objekts `invocationContext` lässt sich der Aufruf von `invokeProcedure` ermitteln, zu dem die Ergebnisse gehören, die gerade ausgeführt werden. |

## Objekt "WL.ClientMessages"
{: #the-wlclientmessages-object }
Sie können eine Liste der Systemnachrichten anzeigen, die im Objekt `WL.ClientMessages` gespeichert sind, und die Übersetzung dieser Systemnachrichten ermöglichen. 

Sie müssen die Systemnachrichten auf einer globalen JavaScript-Ebene überschreiben, da einige Codeabschnitte erst ausgeführt werden, nachdem die Anwendung erfolgreich initialisiert wurde.
{: note}

Im Objekt `WL.ClientMessages` werden die Systemnachrichten gespeichert, die in der Datei **worklight/messages/messages.json** definiert sind. Diese Datei befindet sich im Umgebungsordner der Projekte, die Sie mit {{site.data.keys.product }} generiert haben. Wenn Sie die Übersetzung einer Systemnachricht ermöglichen möchten, müssen Sie den Wert dieser Nachricht im Objekt  `WL.ClientMessages` überschreiben, wie im folgenden Codebeispiel angegeben: 

```javascript
WL.ClientMessages.invalidUsernamePassword="The custom user name and password are not valid";
```
{: code}


* [JavaScript-API-Referenz](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/api/client-side-api/javascript/client/#javascript-api-reference)
* [Objective-C-API-Referenz (für Cordova)](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/api/client-side-api/javascript/client/#objective-c-api-reference-for-cordova)
{: note}
