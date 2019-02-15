---

copyright:
  years: 2018
lastupdated:  "2018-06-18"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

#	JSONStore in Cordova-Anwendungen
{: #cordova_jsonstore}

## Voraussetzungen
{: #prerequisites }
* Lesen Sie die [JSONStore-Übersicht](jsonstore.html).
* Stellen Sie sicher, dass das MobileFirst Cordova-SDK zum Projekt hinzugefügt wurde. Folgen Sie dem Lernprogramm [Mobile Foundation-SDK zu Cordova-Anwendungen hinzufügen![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/cordova/){: new_window}.

## JSONStore hinzufügen
{: #adding-jsonstore }
Gehen Sie wie folgt vor, um ein JSONStore-Plug-in zu Ihrer Cordova-Anwendung hinzuzufügen:

1. Öffnen Sie ein **Befehlszeilenfenster** und navigieren Sie zu Ihrem Cordova-Projektordner.
2. Führen Sie den folgenden Befehl aus: `cordova plugin add cordova-plugin-mfp-jsonstore`.

![JSONStore-Feature hinzufügen](images/jsonstore-add-plugin.png)

## Basisverwendung
{: #basic-usage }
### Initialize
{: #initialize }
Verwenden Sie `init`, um eine oder mehrere JSONStore-Sammlungen zu starten.  

Das Starten oder Bereitstellen einer Sammlung bedeutet, dass der persistente Speicher erstellt wird, der die Sammlung und die Dokumente enthält, falls er nicht vorhanden ist. Wenn der persistente Speicher verschlüsselt und ein korrektes Kennwort übergeben wird, werden die erforderlichen Sicherheitsverfahren ausgeführt, um die Daten zugänglich zu machen.

```javascript
var collections = {
    people : {
        searchFields: {name: 'string', age: 'integer'}
    }
};

WL.JSONStore.init(collections).then(function (collections) {
    // Erfolg behandeln - collection.people (people's collection)
}).fail(function (error) {
    // Fehler behandeln
});
```
{: codeblock}

> Informationen zu den Zusatzfunktionen, die Sie bei der Initialisierung aktivieren können, finden Sie im zweiten Teil dieses Lernprogramms in den Abschnitten **Sicherheit**, **Unterstützung mehrerer Benutzer** und **MobileFirst-Adapterintegration**.

### Get
{: #get }
Verwenden Sie `get`, um einen Zugriffsmechanismus für die Sammlung zu erstellen.Sie müssen `init` vor "get" aufrufen. Andernfalls ist das Ergebnis von `get` nicht definiert.

```javascript
var collectionName = 'people';
var people = WL.JSONStore.get(collectionName);
```
{: codeblock}

Mit der Variablen `people` lassen sich nun Operationen für die Sammlung `people` ausführen, beispielsweise `add`, `find` und `replace`.

### Add
{: #add }
Verwenden Sie `add`, um Daten als Dokumente in einer Sammlung zu speichern.

```javascript
var collectionName = 'people';
var options = {};
var data = {name: 'yoel', age: 23};

WL.JSONStore.get(collectionName).add(data, options).then(function () {
    // Erfolg behandeln
}).fail(function (error) {
    // Fehler behandeln
});
```
{: codeblock}

### Find
{: #find }
* Verwenden Sie `find`, um mithilfe einer Abfrage ein Dokument in einer Sammlung zu lokalisieren.  
* Verwenden Sie `findAll`, um alle Dokumente in einer Sammlung abzurufen.  
* Verwenden Sie `findById`, um nach der eindeutigen Dokumentkennung zu suchen.  

Das Standardverhalten von "find" ist eine "unscharfe" Suche.

```javascript
var query = {name: 'yoel'};
var collectionName = 'people';
var options = {
  exact: false, // Standardwert
  limit: 10 // Rückgabe von maximal 10 Dokumenten. Standardeinstellung: Rückgabe aller Dokumente
};

WL.JSONStore.get(collectionName).find(query, options).then(function (results) {
    // Erfolg behandeln - Ergebnisse (Array mit Dokumenten gefunden)
}).fail(function (error) {
    // Fehler behandeln
});
```
{: codeblock}

```javascript
var age = document.getElementById("findByAge").value || '';

if(age == "" || isNaN(age)){
  alert("Please enter a valid age to find");
}
else {
  query = {age: parseInt(age, 10)};
  var options = {
    exact: true,
    limit: 10 // Rückgabe von maximal 10 Dokumenten.
  };
  WL.JSONStore.get(collectionName).find(query, options).then(function (res) {
    // Erfolg behandeln - Ergebnisse (Array mit Dokumenten gefunden)
  }).fail(function (errorObject) {
    // Fehler behandeln
  });
}
```
{: codeblock}

### Replace
{: #replace }
Verwenden Sie `replace`, um Dokumente in einer Sammlung zu ändern.Zum Ersetzen verwenden Sie das Feld `_id`, die eindeutige Dokumentkennung.

```javascript
var document = {
  _id: 1, json: {name: 'chevy', age: 23}
};
var collectionName = 'people';
var options = {};

WL.JSONStore.get(collectionName).replace(document, options).then(function (numberOfDocsReplaced) {
    // Erfolg behandeln
}).fail(function (error) {
    // Fehler behandeln
});
```
{: codeblock}

Bei diesem Beispiel wird davon ausgegangen, dass sich das Dokument `{_id: 1, json: {name: 'yoel', age: 23} }` in der Sammlung befindet.

### Remove
{: #remove }
Verwenden Sie `remove`, um ein Dokument aus einer Sammlung zu löschen.  
Dokumente werden erst aus der Sammlung entfernt, wenn Sie "push" aufrufen.  

> Weitere Informationen finden Sie unten in diesem Lernprogramm im Abschnitt **MobileFirst-Adapterintegration**.

```javascript
var query = {_id: 1};
var collectionName = 'people';
var options = {exact: true};
WL.JSONStore.get(collectionName).remove(query, options).then(function (numberOfDocsRemoved) {
    // Erfolg behandeln
}).fail(function (error) {
    // Fehler behandeln
});
```
{: codeblock}

### removeCollection
{: #remove-collection }
Verwenden Sie `removeCollection`, um alle Dokumente zu löschen, die in einer Sammlung gespeichert sind. Diese Operation ähnelt dem Löschen einer Tabelle in einer Datenbank.

```javascript
var collectionName = 'people';
WL.JSONStore.get(collectionName).removeCollection().then(function (removeCollectionReturnCode) {
    // Erfolg behandeln
}).fail(function (error) {
    // Fehler behandeln
});
```
{: codeblock}

## Erweiterte Verwendung
{: #advanced-usage }
### Destroy
{: #destory }
Verwenden Sie `destroy`, um die folgenden Daten zu entfernen:

* Alle Dokumente
* Alle Sammlungen
* Alle Speicher (siehe "**Unterstützung mehrerer Benutzer**" unten in diesem Lernprogramm)
* Alle JSONStore-Metadaten und -Sicherheitsartefakte (siehe Abschnitt "**Sicherheit**" unten in diesem Lernprogramm)

```javascript
var collectionName = 'people';
WL.JSONStore.destroy().then(function () {
    // Erfolg behandeln
}).fail(function (error) {
    // Fehler behandeln
});
```
{: codeblock}

### Sicherheit
{: #security }
Sie können alle Sammlungen in einem Speicher sichern, indem Sie ein Kennwort an die Funktion `init` übergeben. Wenn kein Kennwort übergeben wird, werden die Dokumente aller Sammlungen im Speicher nicht verschlüsselt.

Die Datenverschlüsselung ist nur in Android-, iOS-, Windows 8.1 Universal- und Windows 10 UWP-Umgebungen verfügbar.  
Einige Sicherheitsmetadaten werden in der *Schlüsselkette* (iOS), den *gemeinsam genutzten Vorgaben* (Android) oder dem *Fach für Berechtigungsnachweise* (Windows 8.1) gespeichert.  
Der Speicher wird mit einem 256-Bit-AES-Schlüssel (AES - Advanced Encryption Standard) verschlüsselt. Alle Schlüssel werden mit Password-Based Key Derivation Function 2 (PBKDF2) verstärkt.

Sperren Sie mit `closeAll` den Zugriff auf alle Sammlungen, bis Sie `init` erneut aufrufen. Wenn Sie sich `init` als eine Anmeldefunktion vorstellen, können Sie `closeAll` als die entsprechende Abmeldefunktion betrachten. Ändern Sie das Kennwort mit `changePassword`.

```javascript
var collections = {
  people: {
    searchFields: {name: 'string'}
  }
};
var options = {password: '123'};
WL.JSONStore.init(collections, options).then(function () {
    // Erfolg behandeln
}).fail(function (error) {
    // Fehler behandeln
});
```
{: codeblock}

#### Verschlüsselung
{: #encryption }
*nur iOS*. Standardmäßig verwendet das MobileFirst Cordova-SDK für iOS zur Verschlüsselung APIs, die von iOS bereitgestellt werden. Gehen Sie wie folgt vor, wenn Sie diese lieber durch OpenSSL ersetzen möchten:

1. Fügen sie das Plug-in "cordova-plugin-mfp-encrypt-utils" hinzu: `cordova plugin add cordova-plugin-mfp-encrypt-utils`.
2. Verwenden Sie in der Anwendungslogik `WL.SecurityUtils.enableNativeEncryption(false)`, um die Option für OpenSSL zu aktivieren.

### Unterstützung mehrerer Benutzer
{: #multiple-user-support }
Sie können in einer einzigen MobileFirst-Anwendung mehrere Speicher erstellen, die verschiedene Sammlungen enthalten.Die Funktion `init` kann ein Optionsobjekt mit einem Benutzernamen annehmen. Wenn kein Benutzername vergeben wird, lautet der Standardbenutzername **jsonstore**.

```javascript
var collections = {
  people: {
    searchFields: {name: 'string'}
  }
};
var options = {username: 'yoel'};
WL.JSONStore.init(collections, options).then(function () {
    // Erfolg behandeln
}).fail(function (error) {
    // Fehler behandeln
});
```
{: codeblock}

### MobileFirst-Adapterintegration
{: #mobilefirst-adapter-integration }
In diesem Abschnitt wird davon ausgegangen, dass Sie sich mit Adaptern auskennen.  

Die Adapterintegration ist optional und bietet Möglichkeiten zum Senden von Daten aus einer Sammlung an einen Adapter und zum Abrufen von Daten aus einem Adapter in eine Sammlung.  
Sie können diese Ziele mithilfe von `WLResourceRequest` oder `jQuery.ajax` erreichen, wenn Sie mehr Flexibilität benötigen.

### Adapterimplementierung
{: #adapter-implementation }
Erstellen Sie einen Adapter mit dem Namen "**JSONStoreAdapter**".  
Definieren Sie seine Prozeduren `addPerson`, `getPeople`, `pushPeople`, `removePerson` und `replacePerson`.

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
{: #load-data-from-an-adapter }
Verwenden Sie `WLResourceRequest`, um Daten von einem Adapter zu laden.

```javascript
try {
     var resource = new WLResourceRequest("adapters/JSONStoreAdapter/getPeople", WLResourceRequest.GET);
     resource.send()
     .then(function (responseFromAdapter) {
          var data = responseFromAdapter.responseJSON.peopleList;
     },function(err){
      	//Fehler behandeln
     });
} catch (e) {
    alert("Failed to load data from adapter " + e.Messages);
}
```
{: codeblock}

#### getPushRequired (vorläufige Dokumente)
{: #get-push-required-dirty-documents }
Beim Aufruf von `getPushRequired` wird ein Array so genannter *"vorläufiger Dokumente"* zurückgegeben. Dies sind Dokumente mit lokalen Änderungen, die auf dem Back-End-System nicht vorhanden sind. Diese Dokumente werden an den Adapter gesendet, wenn  `push` aufgerufen wird.

```javascript
var collectionName = 'people';
WL.JSONStore.get(collectionName).getPushRequired().then(function (dirtyDocuments) {
    // Erfolg behandeln
}).fail(function (error) {
  // Fehler behandeln
});
```
{: codeblock}

Damit JSONStore die Dokumente nicht als "vorläufig" markiert, müssen Sie die Option `{markDirty:false}` an `add`, `replace` und `remove` übergeben.

Sie können auch die API  ` getAllDirty `  verwenden, um die schmutzigen Dokumente abzurufen:

```javascript
WL.JSONStore.get(collectionName).getAllDirty()
.then(function (dirtyDocuments) {
    // Erfolg behandeln
}).fail(function (errorObject) {
    // Fehler behandeln
});
```
{: codeblock}

#### Änderungen mit Push übergeben
{: #push }
Wenn Sie Änderungen per Push-Operation an einen Adapter übergeben wollen, müssen Sie `getAllDirty` aufrufen, um eine Liste der Dokumente mit Änderungen abzurufen, und danach `WLResourceRequest` verwenden. Nachdem die Daten gesendet wurden und eine erfolgreiche Antwort empfangen wurde, müssen Sie `markClean` aufrufen.

```javascript
try {
     var collectionName = "people";
     var dirtyDocs;

     WL.JSONStore.get(collectionName)
     .getAllDirty()
     .then(function (arrayOfDirtyDocuments) {
        dirtyDocs = arrayOfDirtyDocuments;

        var resource = new WLResourceRequest("adapters/JSONStoreAdapter/pushPeople", WLResourceRequest.POST);
        resource.setQueryParameter('params', [dirtyDocs]);
        return resource.send();
     }).then(function (responseFromAdapter) {
        return WL.JSONStore.get(collectionName).markClean(dirtyDocs);
     }).then(function (res) {
	  //Erfolg behandeln
     }).fail(function (errorObject) {
       // Fehler behandeln.
     });

} catch (e) {
    alert("Failed To Push Documents to Adapter");
}
```
{: codeblock}

### Enhance
{: #enhance }
Erweitern Sie die API-Kerndefinitionen mit `enhance` entsprechend Ihren Anforderungen, indem Sie Funktionen zu einem Sammlungsprototyp hinzufügen.
In diesem Beispiel (dem nachstehenden Code-Snippet) wird gezeigt, wie Sie mithilfe von `enhance` die Funktion `getValue` hinzufügen, die sich auf die Sammlung `keyvalue` auswirkt. Sie nimmt als einzigen Parameter `key` (eine Zeichenfolge) an und gibt ein einziges Ergebnis zurück.

```javascript
var collectionName = 'keyvalue';
WL.JSONStore.get(collectionName).enhance('getValue', function (key) {
    var deferred = $.Deferred();
    var collection = this;
    // Exakte Suche nach dem Schlüssel durchführen
    collection.find({key: key}, {exact:true, limit: 1}).then(deferred.resolve, deferred.reject);
    return deferred.promise();
});

//Syntax:
var key = 'myKey';
WL.JSONStore.get(collectionName).getValue(key).then(function (result) {
    // Erfolg behandeln
    // Das Ergebnis ist ein Array mit Dokumenten mit den Ergebnissen der Suche.
}).fail(function () {
    // Fehler behandeln
});
```
{: codeblock}

## Beispielanwendung
{: #sample-application }
Das JSONStoreSwift-Projekt enthält eine Cordova-Anwendung, die das JSONStore-API-Set verwendet.  
Darin ist ein JavaScript-Adapter-Maven-Projekt enthalten.

![JSONStore-Beispielapp](images/jsonstore-cordova.png)

[Klicken Sie, um das](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreCordova/tree/release80) Cordova-Projekt herunterzuladen.  
[Klicken Sie, um das](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreAdapter/tree/release80) Adapter-Maven-Projekt herunterzuladen.  

### Verwendungsbeispiel
{: #sample-usage }
Folgen Sie den Anweisungen in der README.md-Datei des Beispiels.
