---

copyright:
  years: 2018
lastupdated: "2018-11-26"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Configura l'archiviazione offline
{: #configure_offline_storage}

JSONStore Mobile Foundation è un'API lato client facoltativa che fornisce un semplice sistema di archiviazione orientato ai documenti. JSONStore abilita l'archiviazione persistente di documenti JSON. I documenti in un'applicazione sono disponibili in JSONStore anche quando il dispositivo che sta eseguendo l'applicazione è offline. Questa archiviazione persistente e sempre disponibile può essere utile per consentire agli utenti di accedere ai documenti quando, ad esempio, non è disponibile alcuna connessione di rete nel dispositivo.Per una panoramica dei concetti e per la terminologia di JSONStore, vedi [qui](jsonstore.html).

## Configura l'archiviazione offline per le applicazioni Cordova o Iconic
{: #configure_offline_storage_cordova}

Assicurati che l'SDK Mobile Foundation Cordova sia stato aggiunto al progetto. 

Attieniti all'esercitazione [Adding the Mobile Foundation SDK to Cordova applications ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/cordova/).
{: tip}

### Aggiunta di JSONStore al tuo progetto Cordova
{: #adding_jsonstore_cordova}

1. Apri una finestra di riga di comando e vai alla tua cartella del progetto Cordova.
2. Esegui il comando: 
   ```bash
   cordova plugin add cordova-plugin-mfp-jsonstore
   ```

### Inizializza la raccolta JSONStore
{: #initialize_jsonstore_cordova}   

Utilizza `init` per avviare una o più raccolte JSONStore.
Avviare o eseguire il provisioning di una raccolta significa creare l'archiviazione persistente che contiene la raccolta e i documenti, se non esiste. Se viene passata una password a options, l'archiviazione persistente viene crittografata con la password.

```javascript
var collections = {
    people : {
        searchFields: {name: 'string', age: 'integer'}
    }
};

WL.JSONStore.init(collections).then(function (collections) {
    // handle success - collection.people (people's collection)
}).fail(function (error) {
    // handle failure
});
```

### Ottieni un accessor alla tua raccolta JSONStore
{: #get_jsonstore_cordova} 

Utilizza `get` per creare un accessor alla raccolta. Devi richiamare `init` prima di richiamare get, altrimenti il risultato di `get` sarà indefinito (*undefined*).

```javascript
var collectionName = 'people';
var people = WL.JSONStore.get(collectionName);
```
La variabile *people* può ora essere utilizzata per eseguire operazioni sulla raccolta *people* quali `add`, `find` e `replace`.

### Aggiungi documenti a una raccolta
{: #add_jsonstore_cordova} 

Utilizza `add` per archiviare i dati come documenti all'interno di una raccolta.

```javascript
var collectionName = 'people';
var options = {};
var data = {name: 'yoel', age: 23};

WL.JSONStore.get(collectionName).add(data, options).then(function () {
    // handle success
}).fail(function (error) {
    // handle failure
});
```

### Trova documenti all'interno di una raccolta
{: #find_jsonstore_cordova} 

* Utilizza `find` per individuare un documento all'interno di una raccolta utilizzando una query.
* Utilizza `findAll` per richiamare tutti i documenti all'interno di una raccolta.
* Utilizza `findById` per cercare in base all'identificativo univoco del documento.

La modalità di funzionamento predefinita per find consiste nell'eseguire una ricerca "fuzzy".

```javascript
var query = {name: 'yoel'};
var collectionName = 'people';
var options = {
  exact: false, //default
  limit: 10 // returns a maximum of 10 documents, default: return every document
};

WL.JSONStore.get(collectionName).find(query, options).then(function (results) {
    // handle success - results (array of documents found)
}).fail(function (error) {
    // handle failure
});
```

```javascript
var age = document.getElementById("findByAge").value || '';

if(age == "" || isNaN(age)){
  alert("Please enter a valid age to find");
}
else {
  query = {age: parseInt(age, 10)};
  var options = {
    exact: true,
    limit: 10 //returns a maximum of 10 documents
  };
  WL.JSONStore.get(collectionName).find(query, options).then(function (res) {
    // handle success - results (array of documents found)
  }).fail(function (errorObject) {
    // handle failure
  });
}
```

### Sostituisci documenti all'interno di una raccolta
{: #replace_jsonstore_cordova} 

Utilizza `replace` per modificare i documenti all'interno di una raccolta. Il campo che usi per eseguire la sostituzione è `_id`, l'identificativo univoco del documento.

```javascript
var document = {
  _id: 1, json: {name: 'chevy', age: 23}
};
var collectionName = 'people';
var options = {};

WL.JSONStore.get(collectionName).replace(document, options).then(function (numberOfDocsReplaced) {
    // handle success
}).fail(function (error) {
    // handle failure
});
```
Questo esempio presume che il documento `{_id: 1, json: {name: 'yoel', age: 23} }` sia nella raccolta.


### Rimuovi documenti da una raccolta
{: #remove_jsonstore_cordova} 

Utilizza `remove` per eliminare un documento da una raccolta.
I documenti non vengono cancellati dalla raccolta finché non richiami push.

```javascript
var query = {_id: 1};
var collectionName = 'people';
var options = {exact: true};
WL.JSONStore.get(collectionName).remove(query, options).then(function (numberOfDocsRemoved) {
    // handle success
}).fail(function (error) {
    // handle failure
});
```

### Rimuovi un'intera raccolta
{: #remove_collection_jsonstore_cordova} 

Utilizza `removeCollection` per eliminare tutti i documenti archiviati all'interno di una raccolta. Questa operazione è simile all'eliminazione di una tabella in termini del database.

### Elimina permanentemente JSONStore
{: #destroy_jsonstore_cordova} 

Utilizza `destroy` per rimuovere i seguenti dati:
* Tutti i documenti
* Tutte le raccolte
* Tutti gli archivi
* Tutte le risorse utente di sicurezza e tutti i metadati JSONStore 

## Configura l'archiviazione offline per le applicazioni iOS
{: #configure_offline_storage_ios}

Assicurati che l'SDK nativo Mobile Foundation sia stato aggiunto al progetto Xcode. 

Attieniti all'esercitazione [Adding the Mobile Foundation SDK to iOS applications ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/ios/).
{: tip}

### Aggiunta di JSONStore al tuo progetto iOS
{: #adding_jsonstore_ios}

1. Aggiungi quanto segue al `podfile` esistente, alla root del progetto Xcode.
   ```bash
   pod 'IBMMobileFirstPlatformFoundationJSONStore'
   ```
2. Dalla riga di comando, vai alla root del progetto Xcode ed esegui il comando: 
   ```bash
   pod install
   ``` 
3. Ogni volta che vuoi usare JSONStore, assicurati di importare l'intestazione JSONStore:
   **Objective-C**:
   ```objectivec
   #import <IBMMobileFirstPlatformFoundationJSONStore/IBMMobileFirstPlatformFoundationJSONStore.h>
   ``` 
   **Swift:**
   ```swift
   import IBMMobileFirstPlatformFoundationJSONStore
   ```   

### Apri la raccolta JSONStore: iOS
{: #open_ios} 

Utilizza `openCollections` per aprire una o più raccolte JSONStore.

```swift
let collection:JSONStoreCollection = JSONStoreCollection(name: "people")

collection.setSearchField("name", withType: JSONStore_String)
collection.setSearchField("age", withType: JSONStore_Integer)

do {
  try JSONStore.sharedInstance().openCollections([collection], withOptions: nil)
} catch let error as NSError {
  // handle error
}
```

### Ottieni un accessor alla tua raccolta JSONStore
{: #get_jsonstore_ios} 

Utilizza `getCollectionWithName` per creare un accessor alla raccolta. Devi richiamare `openCollections` prima di richiamare `getCollectionWithName`.

```swift
let collectionName:String = "people"
let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)
```
La variabile collection può ora essere utilizzata per eseguire operazioni sulla raccolta `people` quali `add`, `find` e `replace`.

### Aggiungi documenti a una raccolta
{: #add_jsonstore_ios} 

Utilizza `addData` per archiviare i dati come documenti all'interno di una raccolta.

```swift
let collectionName:String = "people"
let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

let data = ["name" : "yoel", "age" : 23]

do  {
  try collection.addData([data], andMarkDirty: true, withOptions: nil)
} catch let error as NSError {
  // handle error
}
```

### Trova documenti all'interno di una raccolta
{: #find_jsonstore_ios} 

Utilizza `findWithQueryParts` per individuare un documento all'interno di una raccolta utilizzando una query. Utilizza `findAllWithOptions` per richiamare tutti i documenti all'interno di una raccolta.Utilizza `findWithIds` per cercare in base all'identificativo univoco del documento.

```swift
let collectionName:String = "people"
let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

let options:JSONStoreQueryOptions = JSONStoreQueryOptions()
// returns a maximum of 10 documents, default: returns every document
options.limit = 10

let query:JSONStoreQueryPart = JSONStoreQueryPart()
query.searchField("name", like: "yoel")

do  {
  let results:NSArray = try collection.findWithQueryParts([query], andOptions: options)
} catch let error as NSError {
  // handle error
}
```

### Sostituisci documenti all'interno di una raccolta
{: #replace_jsonstore_ios} 

Utilizza `replaceDocuments` per modificare i documenti all'interno di una raccolta. Il campo che usi per eseguire la sostituzione è `_id`, l'identificativo univoco del documento.

```swift
let collectionName:String = "people"
let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

var document:Dictionary<String,AnyObject> = Dictionary()
document["name"] = "chevy"
document["age"] = 23

var replacement:Dictionary<String, AnyObject> = Dictionary()
replacement["_id"] = 1
replacement["json"] = document

do {
  try collection.replaceDocuments([replacement], andMarkDirty: true)
} catch let error as NSError {
  // handle error
}
```
Questo esempio presume che il documento `{_id: 1, json: {name: 'yoel', age: 23} }` sia nella raccolta.

### Rimuovi documenti da una raccolta
{: #remove_jsonstore_ios} 

Utilizza `removeWithIds` per eliminare un documento da una raccolta. I documenti non vengono cancellati dalla raccolta finché non richiami `markDocumentClean`. 

```swift
let collectionName:String = "people"
let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

do {
  try collection.removeWithIds([1], andMarkDirty: true)
} catch let error as NSError {
  // handle error
}
```

### Rimuovi un'intera raccolta
{: #remove_collection_jsonstore_ios} 

Utilizza `removeCollection` per eliminare tutti i documenti archiviati all'interno di una raccolta. Questa operazione è simile all'eliminazione di una tabella in termini del database.

```swift
let collectionName:String = "people"
let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

do {
  try collection.removeCollection()
} catch let error as NSError {
  // handle error
}
```

### Elimina permanentemente JSONStore
{: #destroy_jsonstore_ios} 

Utilizza `destroyData` per rimuovere i seguenti dati:
* Tutti i documenti
* Tutte le raccolte
* Tutti gli archivi
* Tutte le risorse utente di sicurezza e tutti i metadati JSONStore 

```swift
do {
  try JSONStore.sharedInstance().destroyData()
} catch let error as NSError {
  // handle error
}
```

## Configura l'archiviazione offline per le applicazioni Android
{: #configure_offline_storage_android}

Assicurati che l'SDK nativo Mobile Foundation sia stato aggiunto al progetto Android Studio. 

Attieniti all'esercitazione [Adding the Mobile Foundation SDK to Android applications ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/android/).
{: tip}

### Aggiunta di JSONStore al tuo progetto Android
{: #adding_jsonstore_android}

1. Da **Android → Gradle Scripts**, seleziona il file `build.gradle (Module: app)`.
2. Aggiungi quanto segue alla sezione `dependencies` esistente: 
   ```bash
   compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundationjsonstore:8.0.+'
   ``` 
3. Aggiungi quanto segue alla sezione `DefaultConfig` del tuo file `build.gradle`:
   ```gradle
   ndk {
     abiFilters "armeabi", "armeabi-v7a", "x86", "mips"
   }
   ``` 
   Aggiungiamo abiFilters per garantire che le applicazioni con JSONStore verranno eseguite in una qualsiasi delle architetture sopra specificate. Ciò è obbligatorio poiché JSONStore dipende da una libreria di terze parti che supporta solo queste architetture.
   {: note}


### Apri la raccolta JSONStore: Android
{: #open_android} 

Utilizza `openCollections` per aprire una o più raccolte JSONStore.

```java
Context context = getContext();
try {
  JSONStoreCollection people = new JSONStoreCollection("people");
  people.setSearchField("name", SearchFieldType.STRING);
  people.setSearchField("age", SearchFieldType.INTEGER);
  List<JSONStoreCollection> collections = new LinkedList<JSONStoreCollection>();
  collections.add(people);
  WLJSONStore.getInstance(context).openCollections(collections);
  // handle success
} catch(JSONStoreException e) {
  // handle failure
}
```

### Ottieni un accessor alla tua raccolta JSONStore
{: #get_jsonstore_android} 

Utilizza `getCollectionByName` per creare un accessor alla raccolta. Devi richiamare `openCollections` prima di richiamare `getCollectionByName`.

```java
Context context = getContext();
try {
  String collectionName = "people";
  JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName);
  // handle success
} catch(JSONStoreException e) {
  // handle failure
}
```
La variabile collection può ora essere utilizzata per eseguire operazioni sulla raccolta `people` quali `add`, `find` e `replace`.

### Aggiungi documenti a una raccolta
{: #add_jsonstore_android} 

Utilizza `addData` per archiviare i dati come documenti all'interno di una raccolta.

```java
Context context = getContext();
try {
  String collectionName = "people";
  JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName);
  //Add options.
  JSONStoreAddOptions options = new JSONStoreAddOptions();
  options.setMarkDirty(true);
  JSONObject data = new JSONObject("{age: 23, name: 'yoel'}")
  collection.addData(data, options);
  // handle success
} catch(JSONStoreException e) {
  // handle failure
}
```

### Trova documenti all'interno di una raccolta
{: #find_jsonstore_android} 

Utilizza `findDocuments` per individuare un documento all'interno di una raccolta utilizzando una query. Utilizza `findAllDocuments` per richiamare tutti i documenti all'interno di una raccolta. Utilizza `findDocumentById` per cercare in base all'identificativo univoco del documento.

```java
Context context = getContext();
try {
  String collectionName = "people";
  JSONStoreQueryPart queryPart = new JSONStoreQueryPart();
  // fuzzy search LIKE
  queryPart.addLike("name", name);
  JSONStoreQueryParts query = new JSONStoreQueryParts();
  query.addQueryPart(queryPart);
  JSONStoreFindOptions options = new JSONStoreFindOptions();
  // returns a maximum of 10 documents, default: returns every document
  options.setLimit(10);
  JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName);
  List<JSONObject> results = collection.findDocuments(query, options);
  // handle success
} catch(JSONStoreException e) {
  // handle failure
}
```

### Sostituisci documenti all'interno di una raccolta
{: #replace_jsonstore_android} 

Utilizza `replaceDocuments` per modificare i documenti all'interno di una raccolta. Il campo che usi per eseguire la sostituzione è `_id`, l'identificativo univoco del documento.

```java
Context context = getContext();
try {
  String collectionName = "people";
  JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName);
  JSONStoreReplaceOptions options = new JSONStoreReplaceOptions();
  // mark data as dirty
  options.setMarkDirty(true);
  JSONStore replacement = new JSONObject("{_id: 1, json: {age: 23, name: 'chevy'}}");
  collection.replaceDocument(replacement, options);
  // handle success
} catch(JSONStoreException e) {
  // handle failure
}
```
Questo esempio presume che il documento `{_id: 1, json: {name: 'yoel', age: 23} }` sia nella raccolta.

### Rimuovi documenti da una raccolta
{: #remove_jsonstore_android} 

Utilizza `removeDocumentById` per eliminare un documento da una raccolta. I documenti non vengono cancellati dalla raccolta finché non richiami `markDocumentClean`. 

```java
Context context = getContext();
try {
  String collectionName = "people";
  JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName);
  JSONStoreRemoveOptions options = new JSONStoreRemoveOptions();
  // Mark data as dirty
  options.setMarkDirty(true);
  collection.removeDocumentById(1, options);
  // handle success
} catch(JSONStoreException e) {
  // handle failure
}
```

### Rimuovi un'intera raccolta
{: #remove_collection_jsonstore_android} 

Utilizza `removeCollection` per eliminare tutti i documenti archiviati all'interno di una raccolta. Questa operazione è simile all'eliminazione di una tabella in termini del database.

```java
Context context = getContext();
try {
  String collectionName = "people";
  JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName);
  collection.removeCollection();
  // handle success
} catch(JSONStoreException e) {
  // handle failure
}
```

### Elimina permanentemente JSONStore
{: #destroy_jsonstore_android} 

Utilizza `destroy` per rimuovere i seguenti dati:
* Tutti i documenti
* Tutte le raccolte
* Tutti gli archivi
* Tutte le risorse utente di sicurezza e tutti i metadati JSONStore 

```java
Context context = getContext();
try {
  WLJSONStore.getInstance(context).destroy();
  // handle success
} catch(JSONStoreException e) {
  // handle failure
}
```

## Configurazione avanzata dell'archiviazione offline
{: #advanced_offline_storage}

Per la configurazione avanzata dell'archiviazione offline, vedi [qui](advanced_jsonstore.html).
