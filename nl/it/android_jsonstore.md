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

#	JSONStore nelle applicazioni Android
{: #android_jsonstore}

## Prerequisiti
{: #prerequisites }

* Leggi la [panoramica di JSONStore](jsonstore.html).
* Assicurati che al progetto Android Studio sia stato aggiunto l'SDK nativo MobileFirst. Attieniti all'esercitazione [Adding the MobileFirst SDK to Android applications ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/android/).

## Aggiunta di JSONStore
{: #adding-jsonstore }
1. In **Android → Gradle Scripts**, seleziona il file **build.gradle (Module: app)**.

2. Aggiungi quanto segue alla sezione `dependencies` esistente:
```
compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundationjsonstore:8.0.+'
```
{: codeblock}
3. Aggiungi quanto segue alla sezione **DefaultConfig** del tuo file `build.gradle`.
```
  ndk {
        abiFilters "armeabi", "armeabi-v7a", "x86", "mips"
      }
 ```     
 {: codeblock}
 > **Nota** : aggiungiamo *abiFilters* per garantire che le applicazioni con JSONStore verranno eseguite in una qualsiasi delle architetture specificate. L'aggiunta di *abiFilters* è obbligatoria poiché JSONStore dipende da una libreria di terze parti, che supporta solo queste architetture.

## Utilizzo di base
{: #basic-usage }
### Apri (open)
{: #open }
Utilizza `openCollections` per aprire una o più raccolte JSONStore.

Avviare o eseguire il provisioning di una raccolta significa creare l'archiviazione persistente che contiene la raccolta e i documenti, se non esiste. Se l'archiviazione persistente è crittografata e viene passata una password corretta, vengono eseguite le procedure di sicurezza necessarie per rendere i dati accessibili.

Per le funzioni facoltative che puoi abilitare al momento dell'inizializzazione, vedi **Sicurezza, Supporto di più utenti** e **Integrazione dell'adattatore MobileFirst** nella seconda parte di questa esercitazione.

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
{: codeblock}

### Ottieni (get)
{: #get }
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
{: codeblock}

La variabile `collection` può ora essere utilizzata per eseguire operazioni sulla raccolta `people` quali `add`, `find` e `replace`

### Aggiungi (add)
{: #add }
Utilizza `addData` per archiviare i dati come documenti all'interno di una raccolta

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
{: codeblock}

### Trova (find)
{: #find }
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
{: codeblock}

### Sostituisci (replace)
{: #replace }
Utilizza `replaceDocument` per modificare i documenti all'interno di una raccolta. Il campo che usi per eseguire la sostituzione è `_id,`, l'identificativo univoco del documento.

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
{: codeblock}

Questo esempio presume che il documento `{_id: 1, json: {name: 'yoel', age: 23} }` sia nella raccolta.

### Rimuovi (remove)
{: #remove }
Utilizza `removeDocumentById` per eliminare un documento da una raccolta.
I documenti non vengono cancellati dalla raccolta finché non richiami `markDocumentClean`. Per ulteriori informazioni, vedi la sezione **Integrazione dell'adattatore MobileFirst**.

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
{: codeblock}

### Rimuovi raccolta (removeCollection)
{: #remove-collection }
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
{: codeblock}

### Elimina permanentemente (destroy)
{: #destroy }
Utilizza `destroy` per rimuovere i seguenti dati:

* Tutti i documenti
* Tutte le raccolte
* Tutti gli archivi - vedi **Supporto di più utenti** più avanti in questa esercitazione
* Tutte le risorse utente di sicurezza e tutti i metadati JSONStore - vedi **Sicurezza** più avanti in questa esercitazione

```java
Context context = getContext();
try {
  WLJSONStore.getInstance(context).destroy();
  // handle success
} catch(JSONStoreException e) {
  // handle failure
}
```
{: codeblock}

## Utilizzo avanzato
{: #advanced-usage }
### Sicurezza
{: #security }
Puoi proteggere tutte le raccolte in un archivio passando un oggetto `JSONStoreInitOptions` con una password alla funzione `openCollections`. Se non viene passata alcuna password, i documenti di tutte le raccolte nell'archivio non sono crittografati.

Alcuni metadati di sicurezza sono archiviati nelle preferenze condivise (Android).  
L'archivio è crittografato con una chiave AES (Advanced Encryption Standard) a 256 bit. Tutte le chiavi sono rafforzate con PBKDF2 (Password-Based Key Derivation Function 2).

Utilizza `closeAll` per bloccare l'accesso a tutte le raccolte finché non richiami nuovamente `openCollections`. Se pensi a `openCollections` come a un accessor, puoi pensare a `closeAll` come alla sua funzione di disconnessione corrispondente.

Utilizza `changePassword` per modificare la password.

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
  // handle success
} catch(JSONStoreException e) {
  // handle failure
}
```
{: codeblock}

#### Supporto di più utenti
{: #multiple-user-support }
Puoi creare molti archivi che consistono in raccolte differenti in una singola applicazione MobileFirst. La funzione `openCollections` può prendere un oggetto options con un nome utente. Se non viene fornito un nome utente, il nome utente predefinito è "**jsonstore**".

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
  // handle success
} catch(JSONStoreException e) {
  // handle failure
}
```
{: codeblock}

#### Integrazione dell'adattatore MobileFirst
{: #mobilefirst-adapter-integration }
Questa sezione presume che tu abbia dimestichezza con gli adattatori. L'integrazione dell'adattatore è facoltativa e fornire i modi per inviare i dati da una raccolta a un adattatore e ottenere i dati da un adattatore in una raccolta.
Se hai bisogno di maggiore flessibilità, puoi anche raggiungere questi obiettivi utilizzando funzioni quali `WLResourceRequest` oppure la tua istanza di un `HttpClient`.

#### Implementazione dell'adattatore
{: #adapter-implementation }
Crea un adattatore e denominalo "**JSONStoreAdapter**". Definiscine le procedure `addPerson`, `getPeople`, `pushPeople`, `removePerson` e `replacePerson`.

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

#### Carica i dati dall'adattatore MobileFirst
{: #load-data-from-mobilefirst-adapter }
Per caricare i dati da un adattatore, utilizza `WLResourceRequest`.

```java
WLResponseListener responseListener = new WLResponseListener() {
  @Override
  public void onFailure(final WLFailResponse response) {
    // handle failure
  }
  @Override
  public void onSuccess(WLResponse response) {
    try {
      JSONArray loadedDocuments = response.getResponseJSON().getJSONArray("peopleList");
    } catch(Exception e) {
      // error decoding JSON data
    }
  }
};

try {
  WLResourceRequest request = new WLResourceRequest(new URI("/adapters/JSONStoreAdapter/getPeople"), WLResourceRequest.GET);
  request.send(responseListener);
} catch (URISyntaxException e) {
  // handle error
}
```
{: codeblock}

#### getPushRequired (documenti dirty)
{: #get-push-required-dirty-documents }
Il richiamo di `findAllDirtyDocuments` restituisce un array di cosiddetti "documenti dirty", che sono documenti che hanno delle modifiche locali che non esistono sul sistema di back-end.

```java
Context  context = getContext();
try {
  String collectionName = "people";
  JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName);
  List<JSONObject> dirtyDocs = collection.findAllDirtyDocuments();
  // handle success
} catch(JSONStoreException e) {
  // handle failure
}
```
{: codeblock}

Per evitare che JSONStore contrassegni i documenti come "dirty", passa l'opzione `options.setMarkDirty(false)` a `add`, `replace` e `remove`.

#### Esegui il push delle modifiche
{: #push-changes }
Per eseguire il push delle modifiche a un adattatore, richiama `findAllDirtyDocuments` per ottenere un elenco dei documenti con le modifiche e usa quindi `WLResourceRequest`. Dopo che i dati sono stati inviati e che è stata ricevuta una risposta di esito positivo, assicurati di richiamare `markDocumentsClean`.

```java
WLResponseListener responseListener = new WLResponseListener() {
  @Override
  public void onFailure(final WLFailResponse response) {
    // handle failure
  }
  @Override
  public void onSuccess(WLResponse response) {
    // handle success
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
  // handle failure
} catch (URISyntaxException e) {
  // handle error
}
```
{: codeblock}

## Applicazione di esempio
{: #sample-application }
Il progetto `JSONStoreAndroid` contiene un'applicazione Android nativa che utilizza l'API JSONStore.
È incluso un progetto maven dell'adattatore Javascript.

![Immagine dell'applicazione di esempio](images/android-native-screen.jpg)

[Fai clic per scaricare ](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreAndroid) il progetto Android nativo.  
[Fai clic per scaricare](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreAdapter/tree/release80) il progetto Maven dell'adattatore.  

### Utilizzo di esempio
{: #sample-usage }
Segui le istruzioni del file README.md dell'esempio.
