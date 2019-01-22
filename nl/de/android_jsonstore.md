---

copyright:
  years: 2018
lastupdated:  "2018-11-19"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

#	JSONStore in Android-Anwendungen
{: #android_jsonstore}

## Voraussetzungen
{: #prerequisites }

* Lesen Sie die [JSONStore-Übersicht](jsonstore.html).
* Stellen Sie sicher, dass das MobileFirst Native-SDK zum Android Studio-Projekt hinzugefügt wurde. Folgen Sie dem Lernprogramm [MobileFirst-Foundation-SDK zu Android-Anwendungen hinzufügen![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/android/).

## JSONStore hinzufügen
{: #adding-jsonstore }
1. Wählen Sie unter **Android → Gradle-Scripts** die Datei **build.gradle (Module: app)** aus.

2. Fügen Sie zum Bereich `dependencies` Folgendes hinzu:
```
compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundationjsonstore:8.0.+'
```
{: codeblock}
3. Fügen Sie zum Bereich **DefaultConfig** Ihrer Datei `build.gradle` Folgendes hinzu.
```
  ndk {
        abiFilters "armeabi", "armeabi-v7a", "x86", "mips"
      }
 ```     
 {: codeblock}
 > **Hinweis**: *abiFilters* wird hinzugefügt, um sicherzustellen, dass die Apps mit JSONStore in allen angegebenen Architekturen funktionieren. Das Hinzufügen von *abiFilters* ist erforderlich, da JSONStore von einer Bibliothek eines anderen Anbieters abhängig ist, die nur diese Architekturen unterstützt.

## Basisverwendung
{: #basic-usage }
### Open
{: #open }
Verwenden Sie `openCollections`, um eine oder mehrere JSONStore-Sammlungen zu öffnen.

Das Starten oder Bereitstellen einer Sammlung bedeutet, dass der persistente Speicher erstellt wird, der die Sammlung und die Dokumente enthält, falls er nicht vorhanden ist. Wenn der persistente Speicher verschlüsselt und ein korrektes Kennwort übergeben wird, werden die erforderlichen Sicherheitsverfahren ausgeführt, um die Daten zugänglich zu machen.

Informationen zu den Zusatzfunktionen, die Sie bei der Initialisierung aktivieren können, finden Sie im zweiten Teil dieses Lernprogramms in den Abschnitten **Sicherheit, Unterstützung mehrerer Benutzer** und **MobileFirst-Adapterintegration**.

```java
Context context = getContext();
try {
  JSONStoreCollection people = new JSONStoreCollection("people");
  people.setSearchField("name", SearchFieldType.STRING);
  people.setSearchField("age", SearchFieldType.INTEGER);
  List<JSONStoreCollection> collections = new LinkedList<JSONStoreCollection>();
  collections.add(people);
  WLJSONStore.getInstance(context).openCollections(collections);
  // Erfolg behandeln
} catch(JSONStoreException e) {
  // Fehler behandeln
}
```
{: codeblock}

### Get
{: #get }
Verwenden Sie `getCollectionByName`, um einen Zugriffsmechanismus für die Sammlung zu erstellen. Sie müssen `openCollections` vor dem Aufruf von `getCollectionByName` aufrufen.

```java
Context context = getContext();
try {
  String collectionName = "people";
  JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName);
  // Erfolg behandeln
} catch(JSONStoreException e) {
  // Fehler behandeln
}
```
{: codeblock}

Mit der Variablen `collection` lassen sich nun Operationen für die Sammlung `people` ausführen, beispielsweise `add`, `find` und `replace`.

### Add
{: #add }
Verwenden Sie `addData`, um Daten als Dokumente in einer Sammlung zu speichern.

```java
Context context = getContext();
try {
  String collectionName = "people";
  JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName);
  // Optionen hinzufügen.
  JSONStoreAddOptions options = new JSONStoreAddOptions();
  options.setMarkDirty(true);
  JSONObject data = new JSONObject("{age: 23, name: 'yoel'}")
  collection.addData(data, options);
  // Erfolg behandeln
} catch(JSONStoreException e) {
  // Fehler behandeln
}
```
{: codeblock}

### Find
{: #find }
Verwenden Sie `findDocuments`, um mithilfe einer Abfrage ein Dokument in einer Sammlung zu lokalisieren. Verwenden Sie `findAllDocuments`, um alle Dokumente in einer Sammlung abzurufen. Verwenden Sie `findDocumentById`, um nach der eindeutigen Dokumentkennung zu suchen.

```java
Context context = getContext();
try {
  String collectionName = "people";
  JSONStoreQueryPart queryPart = new JSONStoreQueryPart();
  // unscharfe Suche LIKE
  queryPart.addLike("name", name);
  JSONStoreQueryParts query = new JSONStoreQueryParts();
  query.addQueryPart(queryPart);
  JSONStoreFindOptions options = new JSONStoreFindOptions();
  // Rückgabe von maximal 10 Dokumenten. Standardeinstellung: Rückgabe aller Dokumente
  options.setLimit(10);
  JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName);
  List<JSONObject> results = collection.findDocuments(query, options);
  // Erfolg behandeln
} catch(JSONStoreException e) {
  // Fehler behandeln
}
```
{: codeblock}

### Replace
{: #replace }
Verwenden Sie `replaceDocument`, um Dokumente in einer Sammlung zu ändern. Zum Ersetzen verwenden Sie das Feld `_id`, die eindeutige Dokumentkennung. 

```java
Context context = getContext();
try {
  String collectionName = "people";
  JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName);
  JSONStoreReplaceOptions options = new JSONStoreReplaceOptions();
  // Daten als vorläufig markieren
  options.setMarkDirty(true);
  JSONStore replacement = new JSONObject("{_id: 1, json: {age: 23, name: 'chevy'}}");
  collection.replaceDocument(replacement, options);
  // Erfolg behandeln
} catch(JSONStoreException e) {
  // Fehler behandeln
}
```
{: codeblock}

Bei diesem Beispiel wird davon ausgegangen, dass sich das Dokument `{_id: 1, json: {name: 'yoel', age: 23} }` in der Sammlung befindet.

### Remove
{: #remove }
Verwenden Sie `removeDocumentById`, um ein Dokument aus einer Sammlung zu löschen.
Dokumente werden erst aus der Sammlung entfernt, wenn Sie `markDocumentClean` aufrufen. Weitere Informationen finden Sie im Abschnitt **MobileFirst-Adapterintegration**.

```java
Context context = getContext();
try {
  String collectionName = "people";
  JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName);
  JSONStoreRemoveOptions options = new JSONStoreRemoveOptions();
  // Daten als vorläufig markieren
  options.setMarkDirty(true);
  collection.removeDocumentById(1, options);
  // Erfolg behandeln
} catch(JSONStoreException e) {
  // Fehler behandeln
}
```
{: codeblock}

### removeCollection
{: #remove-collection }
Verwenden Sie `removeCollection`, um alle Dokumente zu löschen, die in einer Sammlung gespeichert sind. Diese Operation ähnelt dem Löschen einer Tabelle in einer Datenbank.

```java
Context context = getContext();
try {
  String collectionName = "people";
  JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName);
  collection.removeCollection();
  // Erfolg behandeln
} catch(JSONStoreException e) {
  // Fehler behandeln
}
```
{: codeblock}

### Destroy
{: #destroy }
Verwenden Sie `destroy`, um die folgenden Daten zu entfernen:

* Alle Dokumente
* Alle Sammlungen
* Alle Speicher - siehe Abschnitt **Unterstützung mehrerer Benutzer** unten in diesem Lernprogramm
* Alle JSONStore-Metadaten und -Sicherheitsartefakte - siehe Abschnitt **Sicherheit** unten in diesem Lernprogramm

```java
Context context = getContext();
try {
  WLJSONStore.getInstance(context).destroy();
  // Erfolg behandeln
} catch(JSONStoreException e) {
  // Fehler behandeln
}
```
{: codeblock}

## Erweiterte Verwendung
{: #advanced-usage }
### Sicherheit
{: #security }
Sie können alle Sammlungen in einem Speicher sichern, indem Sie ein `JSONStoreInitOptions`-Objekt mit einem Kennwort an die Funktion `openCollections` übergeben. Wenn kein Kennwort übergeben wird, werden die Dokumente aller Datensammlungen im Geschäft nicht verschlüsselt.

Einige Sicherheitsmetadaten werden in gemeinsam genutzten Vorgaben (Android) gespeichert.
  
Der Speicher wird mit einem 256-Bit-AES-Schlüssel (AES - Advanced Encryption Standard) verschlüsselt. Alle Schlüssel werden mit Password-Based Key Derivation Function 2 (PBKDF2) verstärkt.

Sperren Sie mit `closeAll` den Zugriff auf alle Sammlungen, bis Sie `openCollections` erneut aufrufen. Wenn Sie sich `openCollections` als eine Anmeldefunktion vorstellen, können Sie `closeAll` als die entsprechende Abmeldefunktion betrachten. 

Ändern Sie das Kennwort mit `changePassword`.

```java
Context context = getContext();
try {
  JSONStoreCollection people = new JSONStoreCollection("people");
  people.setSearchField("name", SearchFieldType.STRING);
  people.setSearchField("age", SearchFieldType.INTEGER);
  List<JSONStoreCollection> collections = new LinkedList<JSONStoreCollection>();
  collections.add(people);
  JSONStoreInitOptions options = new JSONStoreInitOptions();
  options.setPassword("123");
  WLJSONStore.getInstance(context).openCollections(collections, options);
  // Erfolg behandeln
} catch(JSONStoreException e) {
  // Fehler behandeln
}
```
{: codeblock}

#### Unterstützung mehrerer Benutzer
{: #multiple-user-support }
Sie können in einer einzigen MobileFirst-Anwendung viele Speicher erstellen, die aus verschiedenen Sammlungen bestehen. Die Funktion `openCollections` kann ein Optionsobjekt mit einem Benutzernamen annehmen. Wenn kein Benutzername angegeben wird, lautet der Standardbenutzername "**jsonstore**".

```java
Context context = getContext();
try {
  JSONStoreCollection people = new JSONStoreCollection("people");
  people.setSearchField("name", SearchFieldType.STRING);
  people.setSearchField("age", SearchFieldType.INTEGER);
  List<JSONStoreCollection> collections = new LinkedList<JSONStoreCollection>();
  collections.add(people);
  JSONStoreInitOptions options = new JSONStoreInitOptions();
  options.setUsername("yoel");
  WLJSONStore.getInstance(context).openCollections(collections, options);
  // Erfolg behandeln
} catch(JSONStoreException e) {
  // Fehler behandeln
}
```
{: codeblock}

#### MobileFirst-Adapterintegration
{: #mobilefirst-adapter-integration }
In diesem Abschnitt wird davon ausgegangen, dass Sie sich mit Adaptern auskennen. Die Adapterintegration ist optional und bietet Möglichkeiten zum Senden von Daten aus einer Sammlung an einen Adapter und zum Abrufen von Daten aus einem Adapter in eine Sammlung. Wenn Sie mehr Flexibilität benötigen, können Sie diese Ziele auch mithilfe von Funktionen wie `WLResourceRequest` oder mit Ihrer eigenen Instanz von `HttpClient` erreichen.


#### Adapterimplementierung
{: #adapter-implementation }
Erstellen Sie einen Adapter mit dem Namen "**JSONStoreAdapter**". Definieren Sie seine Prozeduren `addPerson`, `getPeople`, `pushPeople`, `removePerson` und `replacePerson`.

```javascript
function getPeople() {
	var data = { peopleList : [{name: 'chevy', age: 23}, {name: 'yoel', age: 23}] };
	WL.Logger.debug('Adapter: people, procedure: getPeople called.');
	WL.Logger.debug('Sending data: ' + JSON.stringify(data));
	return data;
}
function pushPeople(data) {
	WL.Logger.debug('Adapter: people, procedure: pushPeople called.');
	WL.Logger.debug('Got data from JSONStore to ADD: ' + data);
	return;
}
function addPerson(data) {
	WL.Logger.debug('Adapter: people, procedure: addPerson called.');
	WL.Logger.debug('Got data from JSONStore to ADD: ' + data);
	return;
}
function removePerson(data) {
	WL.Logger.debug('Adapter: people, procedure: removePerson called.');
	WL.Logger.debug('Got data from JSONStore to REMOVE: ' + data);
	return;
}
function replacePerson(data) {
	WL.Logger.debug('Adapter: people, procedure: replacePerson called.');
	WL.Logger.debug('Got data from JSONStore to REPLACE: ' + data);
	return;
}
```
{: codeblock}

#### Daten aus MobileFirst-Adapter laden
{: #load-data-from-mobilefirst-adapter }
Verwenden Sie `WLResourceRequest`, um Daten von einem Adapter zu laden.

```java
WLResponseListener responseListener = new WLResponseListener() {
  @Override
  public void onFailure(final WLFailResponse response) {
    // Fehler behandeln
  }
  @Override
  public void onSuccess(WLResponse response) {
    try {
      JSONArray loadedDocuments = response.getResponseJSON().getJSONArray("peopleList");
    } catch(Exception e) {
      // Fehler beim Decodieren von JSON-Daten
    }
  }
};

try {
  WLResourceRequest request = new WLResourceRequest(new URI("/adapters/JSONStoreAdapter/getPeople"), WLResourceRequest.GET);
  request.send(responseListener);
} catch (URISyntaxException e) {
  // Fehler behandeln
}
```
{: codeblock}

#### getPushRequired (vorläufige Dokumente)
{: #get-push-required-dirty-documents }
Beim Aufruf von `findAllDirtyDocuments` wird ein Array so genannter "vorläufiger Dokumente" zurückgegeben. Dies sind Dokumente mit lokalen Änderungen, die auf dem Back-End-System nicht vorhanden sind. 

```java
Context  context = getContext();
try {
  String collectionName = "people";
  JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName);
  List<JSONObject> dirtyDocs = collection.findAllDirtyDocuments();
  // Erfolg behandeln
} catch(JSONStoreException e) {
  // Fehler behandeln
}
```
{: codeblock}

Damit JSONStore die Dokumente nicht als "vorläufig" markiert, müssen Sie die Option `options.setMarkDirty(false)` an `add`, `replace` und `remove` übergeben.

#### Änderungen mit Push übergeben
{: #push-changes }
Wenn Sie Änderungen per Push-Operation an einen Adapter übergeben wollen, müssen Sie `findAllDirtyDocuments` aufrufen, um eine Liste der Dokumente mit Änderungen abzurufen, und danach `WLResourceRequest` verwenden. Nachdem die Daten gesendet wurden und eine erfolgreiche Antwort empfangen wurde, müssen Sie `markDocumentsClean` aufrufen.

```java
WLResponseListener responseListener = new WLResponseListener() {
  @Override
  public void onFailure(final WLFailResponse response) {
    // Fehler behandeln
  }
  @Override
  public void onSuccess(WLResponse response) {
    // Erfolg behandeln
  }
};
Context context = getContext();

try {
  String collectionName = "people";
  JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName);
  List<JSONObject> dirtyDocuments = people.findAllDirtyDocuments();

  JSONObject payload = new JSONObject();
  payload.put("people", dirtyDocuments);

  WLResourceRequest request = new WLResourceRequest(new URI("/adapters/JSONStoreAdapter/pushPeople"), WLResourceRequest.POST);
  request.send(payload, responseListener);
} catch(JSONStoreException e) {
  // Fehler behandeln
} catch (URISyntaxException e) {
  // Fehler behandeln
}
```
{: codeblock}

## Beispielanwendung
{: #sample-application }
Das Projekt `JSONStoreAndroid` enthält eine native Android-Anwendung, die die JSONStore-API verwendet. Darin ist ein JavaScript-Adapter-Maven-Projekt enthalten.

![Abbildung der Beispielanwendung](images/android-native-screen.jpg)

[Klicken Sie, um das](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreAndroid) native Android-Projekt herunterzuladen.  
[Klicken Sie, um das](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreAdapter/tree/release80) Adapter-Maven-Projekt herunterzuladen.  

### Verwendungsbeispiel
{: #sample-usage }
Folgen Sie den Anweisungen in der README.md-Datei des Beispiels.
