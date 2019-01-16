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

#	JSONStore nelle applicazioni Cordova
{: #cordova_jsonstore}

## Prerequisiti
{: #prerequisites }
* Leggi la [panoramica di JSONStore](jsonstore.html).
* Assicurati che l'SDK MobileFirst Cordova sia stato aggiunto al progetto. Attieniti all'esercitazione [Adding the Mobile Foundation SDK to Cordova applications ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/cordova/){: new_window}.

## Aggiunta di JSONStore
{: #adding-jsonstore }
Per aggiungere il plug-in JSONStore alla tua applicazione Cordova:

1. Apri una finestra **Riga di comando** e vai alla tua cartella del progetto Cordova.
2. Esegui il comando: `cordova plugin add cordova-plugin-mfp-jsonstore`.

![Aggiungi la funzione JSONStore](images/jsonstore-add-plugin.png)

## Utilizzo di base
{: #basic-usage }
### Inizializza
{: #initialize }
Utilizza `init` per avviare una o più raccolte JSONStore.  

Avviare o eseguire il provisioning di una raccolta significa creare l'archiviazione persistente che contiene la raccolta e i documenti, se non esiste. Se l'archiviazione persistente è crittografata e viene passata una password corretta, vengono eseguite le procedure di sicurezza necessarie per rendere i dati accessibili.

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
{: codeblock}

> Per le funzioni facoltative che puoi abilitare al momento dell'inizializzazione, vedi **Sicurezza**, **Supporto di più utenti**, e **Integrazione dell'adattatore MobileFirst** nella seconda parte di questa esercitazione.

### Ottieni (get)
{: #get }
Utilizza `get` per creare un accessor alla raccolta. Devi richiamare `init` prima di richiamare get, altrimenti il risultato di `get` sarà indefinito.

```javascript
var collectionName = 'people';
var people = WL.JSONStore.get(collectionName);
```
{: codeblock}

La variabile `people` può ora essere utilizzata per eseguire operazioni sulla raccolta `people` quali `add`, `find` e `replace`.

### Aggiungi (add)
{: #add }
Utilizza `add` per archiviare i dati come documenti all'interno di una raccolta

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
{: codeblock}

### Trova (find)
{: #find }
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
    limit: 10 //returns a maximum of 10 documents
  };
  WL.JSONStore.get(collectionName).find(query, options).then(function (res) {
    // handle success - results (array of documents found)
  }).fail(function (errorObject) {
    // handle failure
  });
}
```
{: codeblock}

### Sostituisci (replace)
{: #replace }
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
{: codeblock}

Questo esempio presume che il documento `{_id: 1, json: {name: 'yoel', age: 23} }` sia nella raccolta.

### Rimuovi (remove)
{: #remove }
Utilizza `remove` per eliminare un documento da una raccolta.  
I documenti non vengono cancellati dalla raccolta finché non richiami push.  

> Per ulteriori informazioni, vedi la sezione **Integrazione dell'adattatore MobileFirst** più avanti in questa esercitazione

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
{: codeblock}

### Rimuovi raccolta (removeCollection)
{: #remove-collection }
Utilizza `removeCollection` per eliminare tutti i documenti archiviati all'interno di una raccolta. Questa operazione è simile all'eliminazione di una tabella in termini del database.

```javascript
var collectionName = 'people';
WL.JSONStore.get(collectionName).removeCollection().then(function (removeCollectionReturnCode) {
    // handle success
}).fail(function (error) {
    // handle failure
});
```
{: codeblock}

## Utilizzo avanzato
{: #advanced-usage }
### destroy
{: #destory }
Utilizza `destroy` per rimuovere i seguenti dati:

* Tutti i documenti
* Tutte le raccolte
* Tutti gli archivi (vedi "**Supporto di più utenti**" più avanti in questa esercitazione)
* Tutte le risorse utente di sicurezza e tutti i metadati JSONStore (vedi "**Sicurezza**" più avanti in questa esercitazione)

```javascript
var collectionName = 'people';
WL.JSONStore.destroy().then(function () {
    // handle success
}).fail(function (error) {
    // handle failure
});
```
{: codeblock}

### Sicurezza
{: #security }
Puoi proteggere tutte le tue raccolte in un archivio passando una password alla funzione `init`. Se non viene passata alcuna password, i documenti di tutte le raccolte nell'archivio non saranno crittografati.

La crittografia dei dati è disponibile solo negli ambienti Android, iOS, Windows 8.1 Universal e Windows 10 UWP.  
Alcuni metadati di sicurezza vengono archiviati nel *keychain* (iOS), nelle *preferenze condivise* (Android) o nella *casella di sicurezza delle credenziali* (Windows 8.1).  
L'archivio è crittografato con una chiave AES (Advanced Encryption Standard) a 256 bit. Tutte le chiavi sono rafforzate con PBKDF2 (Password-Based Key Derivation Function 2).

Utilizza `closeAll` per bloccare l'accesso a tutte le raccolte finché non richiami nuovamente `init`. Se pensi a `init` come a un accessor, puoi pensare a `closeAll` come alla sua funzione di disconnessione corrispondente. Utilizza `changePassword` per modificare la password.

```javascript
var collections = {
  people: {
    searchFields: {name: 'string'}
  }
};
var options = {password: '123'};
WL.JSONStore.init(collections, options).then(function () {
    // handle success
}).fail(function (error) {
    // handle failure
});
```
{: codeblock}

#### Crittografia
{: #encryption }
*Solo iOS*. Per impostazione predefinita, l'SDK MobileFirst Cordova per iOS fa affidamento sulle API fornite da iOS per la crittografia. Se preferisci eseguirne la sostituzione con OpenSSL:

1. Aggiungi il plug-in cordova-plugin-mfp-encrypt-utils: `cordova plugin add cordova-plugin-mfp-encrypt-utils`.
2. Nella logica applicativa, utilizza `WL.SecurityUtils.enableNativeEncryption(false)` per abilitare l'opzione OpenSSL.

### Supporto di più utenti
{: #multiple-user-support }
Puoi creare più archivi che contengono raccolte differenti in una singola applicazione MobileFirst. La funzione `init` può prendere un oggetto options con un nome utente. Se non viene dato alcun nome utente, il nome utente predefinito è **jsonstore**.

```javascript
var collections = {
  people: {
    searchFields: {name: 'string'}
  }
};
var options = {username: 'yoel'};
WL.JSONStore.init(collections, options).then(function () {
    // handle success
}).fail(function (error) {
    // handle failure
});
```
{: codeblock}

### Integrazione dell'adattatore MobileFirst
{: #mobilefirst-adapter-integration }
Questa sezione presume che tu abbia dimestichezza con gli adattatori.  

L'integrazione dell'adattatore è facoltativa e fornire i modi per inviare i dati da una raccolta a un adattatore e ottenere i dati da un adattatore in una raccolta.  
Puoi raggiungere questi obiettivi utilizzando `WLResourceRequest` o `jQuery.ajax` se hai bisogno di maggiore flessibilità.

### Implementazione dell'adattatore
{: #adapter-implementation }
Crea un adattatore e denominalo "**JSONStoreAdapter**".   
Definisci le sue procedure `addPerson`, `getPeople`, `pushPeople`, `removePerson` e `replacePerson`.

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
{: #load-data-from-an-adapter }
Per caricare i dati da un adattatore, utilizza `WLResourceRequest`.

```javascript
try {
     var resource = new WLResourceRequest("adapters/JSONStoreAdapter/getPeople", WLResourceRequest.GET);
     resource.send()
     .then(function (responseFromAdapter) {
          var data = responseFromAdapter.responseJSON.peopleList;
     },function(err){
      	//handle failure
     });
} catch (e) {
    alert("Failed to load data from adapter " + e.Messages);
}
```
{: codeblock}

#### getPushRequired (documenti dirty)
{: #get-push-required-dirty-documents }
Il richiamo di `getPushRequired` restituisce un array di cosiddetti *"documenti dirty"*, che sono documenti che hanno delle modifiche locali che non esistono sul sistema di back-end. Questi documenti vengono inviati all'adattatore quando viene richiamato `push`.

```javascript
var collectionName = 'people';
WL.JSONStore.get(collectionName).getPushRequired().then(function (dirtyDocuments) {
    // handle success
}).fail(function (error) {
  // handle failure
});
```
{: codeblock}

   Per evitare che JSONStore contrassegni i documenti come "dirty", passa l'opzione `{markDirty:false}` a `add`, `replace` e `remove`

Puoi anche utilizzare l'API `getAllDirty` per richiamare i documenti dirty:

```javascript
WL.JSONStore.get(collectionName).getAllDirty()
.then(function (dirtyDocuments) {
    // handle success
}).fail(function (errorObject) {
    // handle failure
});
```
{: codeblock}

#### Esegui il push delle modifiche
{: #push }
Per eseguire il push delle modifiche a un adattatore, richiama `getAllDirty` per ottenere un elenco dei documenti con le modifiche e usa quindi `WLResourceRequest`. Dopo che i dati sono stati inviati e che è stata ricevuta una risposta di esito positivo, assicurati di richiamare `markClean`.

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
	  //handle success
     }).fail(function (errorObject) {
       // Handle failure.
     });

} catch (e) {
    alert("Failed To Push Documents to Adapter");
}
```
{: codeblock}

### enhance
{: #enhance }
Utilizza `enhance` per l'estendere l'API core per rispondere alle tue esigenze aggiungendo funzioni a un prototipo di raccolta.
Questo esempio (il frammento di codice di seguito) mostra come utilizzare `enhance` per aggiungere la funzione `getValue` che agisce sulla raccolta `keyvalue`. Prende una `key` (stringa) come suo solo parametro e restituisce un singolo risultato.

```javascript
var collectionName = 'keyvalue';
WL.JSONStore.get(collectionName).enhance('getValue', function (key) {
    var deferred = $.Deferred();
    var collection = this;
    //Do an exact search for the key
    collection.find({key: key}, {exact:true, limit: 1}).then(deferred.resolve, deferred.reject);
    return deferred.promise();
});

//Usage:
var key = 'myKey';
WL.JSONStore.get(collectionName).getValue(key).then(function (result) {
    // handle success
    // result contains an array of documents with the results from the find
}).fail(function () {
    // handle failure
});
```
{: codeblock}

## Applicazione di esempio
{: #sample-application }
Il progetto JSONStoreSwift contiene un'applicazione Cordova che utilizza il set di API JSONStore.  
È incluso un progetto Maven dell'adattatore JavaScript.

![Applicazione di esempio JSONStore](images/jsonstore-cordova.png)

[Fai clic per scaricare](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreCordova/tree/release80) il progetto Cordova.  
[Fai clic per scaricare](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreAdapter/tree/release80) il progetto Maven dell'adattatore.  

### Utilizzo di esempio
{: #sample-usage }
Segui le istruzioni del file README.md dell'esempio.
