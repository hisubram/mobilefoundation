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

# Configuration du stockage hors ligne 
{: #configure_offline_storage}

Mobile Foundation JSONStore est une API côté client facultative qui fournit un système de stockage léger, orienté document. JSONStore active le stockage persistant de documents JSON. Les documents dans une application sont disponibles dans JSONStore même si l'appareil qui exécute l'application est déconnecté. Ce stockage permanent et toujours disponible peut être utile pour donner aux utilisateurs un accès aux documents lorsque, par exemple, aucune connexion réseau n'est disponible sur l'appareil. Pour obtenir un aperçu des concepts et de la terminologie JSONStore, voir [ici](jsonstore.html).

## Configuration du stockage hors connexion pour les applications Cordova ou Ionic 
{: #configure_offline_storage_cordova}

Assurez-vous que le SDK Cordova Mobile Foundation a été ajouté au projet.  

Suivez les instructions du tutoriel [Ajout du SDK Mobile Foundation aux applications Cordova![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/cordova/).
{: tip}

### Ajout de JSONStore à votre projet Cordova 
{: #adding_jsonstore_cordova}

1. Ouvrez une fenêtre de ligne de commande et accédez au dossier de votre projet Cordova. 
2. Exécutez la commande : 
   ```bash
   cordova plugin add cordova-plugin-mfp-jsonstore
   ```

### Initialisation de la collection JSONStore 
{: #initialize_jsonstore_cordova}   

Utilisez `init` pour ouvrir une ou plusieurs collections JSONStore. Démarrer ou mettre à disposition une collection signifie créer le stockage persistant contenant la collection et les documents, s'il n'existe pas. Si un mot de passe est transmis aux options, le stockage persistant est chiffré avec le mot de passe. 

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

### Obtention d'un accesseur à votre collection JSONStore 
{: #get_jsonstore_cordova} 

Utilisez `get` pour créer un accesseur à la collection. Vous devez appeler `init` avant d'appeler get, faute de quoi, le résultat de `get` est *undefined*.

```javascript
var collectionName = 'people';
var people = WL.JSONStore.get(collectionName);
```
La variable *people* peut maintenant être utilisée pour effectuer des opérations sur la collection *people* telles que `add`, `find` et `replace`.

### Ajout de documents à une collection
{: #add_jsonstore_cordova} 

Utilisez `add` pour stocker des données sous forme de documents dans une collection. 

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

### Recherche de documents dans la collection
{: #find_jsonstore_cordova} 

* Utilisez `find` pour localiser un document dans une collection à l'aide d'une requête. 
* Utilisez `findAll` pour extraire tous les documents d'une collection. 
* Utilisez `findById` pour rechercher par l'identifiant unique du document.

Le comportement par défaut pour find consiste à effectuer une recherche "floue". 

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

### Remplacement de documents dans une collection
{: #replace_jsonstore_cordova} 

Utilisez `replace` pour modifier des documents dans une collection. La zone que vous utilisez pour effectuer le remplacement est `_id`, l'identifiant unique du document.

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
Cet exemple suppose que le document `{_id: 1, json: {name: 'yoel', age: 23} }` se trouve dans la collection.


### Suppression de documents d'une collection
{: #remove_jsonstore_cordova} 

Utilisez `remove` pour supprimer un document d'une collection. Les documents ne sont pas effacés de la collection tant que vous n'avez pas appelé push. 

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

### Suppression d'une collection complète
{: #remove_collection_jsonstore_cordova} 

Utilisez `removeCollection` pour supprimer tous les documents stockés dans une collection. Cette opération est similaire à la suppression d'une table en termes de base de données. 

### Destruction de JSONStore
{: #destroy_jsonstore_cordova} 

Utilisez `destroy` pour supprimer les données suivantes :
* Tous les documents
* Toutes les collections
* Tous les magasins
* Toutes les métadonnées et tous les artefacts de sécurité JSONStore 

## Configuration du stockage hors connexion pour les applications iOS
{: #configure_offline_storage_ios}

Assurez-vous que le SDK Mobile Foundation Native a été ajouté au projet Xcode.  

Suivez les instructions du tutoriel [Ajout du SDK Mobile Foundation aux applications iOS![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/ios/).
{: tip}

### Ajout de JSONStore à votre projet iOS 
{: #adding_jsonstore_ios}

1. Ajoutez le code suivant au fichier `podfile` existant, à la racine du projet Xcode.
   ```bash
   pod 'IBMMobileFirstPlatformFoundationJSONStore'
   ```
2. A partir de la ligne de commande, accédez à la racine du projet Xcode et exécutez la commande suivante :  
   ```bash
   pod install
   ``` 
3. Lorsque vous souhaitez utiliser JSONStore, veillez à importer l'en-tête JSONStore :
   **Objective-C**:
   ```objectivec
   #import <IBMMobileFirstPlatformFoundationJSONStore/IBMMobileFirstPlatformFoundationJSONStore.h>
   ``` 
   **Swift :**
   ```swift
   import IBMMobileFirstPlatformFoundationJSONStore
   ```   

### Ouverture de la collection JSONStore : iOS  
{: #open_ios} 

Utilisez `openCollections` pour ouvrir une ou plusieurs collections JSONStore.

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

### Obtention d'un accesseur à votre collection JSONStore 
{: #get_jsonstore_ios} 

Utilisez `getCollectionWithName` pour créer un accesseur à la collection. Vous devez appeler `openCollections` avant d'appeler `getCollectionWithName`.

```swift
let collectionName:String = "people"
    let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

    ```
La collection variable peut maintenant être utilisée pour effectuer des opérations sur la collection `people` telles que `add`, `find` et `replace`.

### Ajout de documents à une collection
{: #add_jsonstore_ios} 

Utilisez `addData` pour stocker des données sous forme de documents dans une collection. 

```swift
let collectionName:String = "people"
let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

let data = ["name" : "yoel", "age" : 23]

do {
  try collection.addData([data], andMarkDirty: true, withOptions: nil)
} catch let error as NSError {
  // handle error
}
```

### Recherche de documents dans la collection
{: #find_jsonstore_ios} 

Utilisez `findWithQueryParts` pour localiser un document dans une collection à l'aide d'une requête. Utilisez `findAllWithOptions` pour extraire tous les documents d'une collection. Utilisez `findWithIds` pour rechercher par l'identifiant unique du document.

```swift
let collectionName:String = "people"
let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

let options:JSONStoreQueryOptions = JSONStoreQueryOptions()
// returns a maximum of 10 documents, default: returns every document
options.limit = 10

let query:JSONStoreQueryPart = JSONStoreQueryPart()
query.searchField("name", like: "yoel")

do {
  let results:NSArray = try collection.findWithQueryParts([query], andOptions: options)
} catch let error as NSError {
  // handle error
}
```

### Remplacement de documents dans une collection
{: #replace_jsonstore_ios} 

Utilisez `replaceDocuments` pour modifier des documents dans une collection. La zone que vous utilisez pour effectuer le remplacement est `_id`, l'identifiant unique du document.

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
Cet exemple suppose que le document `{_id: 1, json: {name: 'yoel', age: 23} }` se trouve dans la collection.

### Suppression de documents d'une collection
{: #remove_jsonstore_ios} 

Utilisez `removeWithIds` pour supprimer un document d'une collection. Les documents ne sont pas effacés de la collection tant que vous n'avez pas appelé `markDocumentClean`.  

```swift
let collectionName:String = "people"
let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

do {
  try collection.removeWithIds([1], andMarkDirty: true)
} catch let error as NSError {
  // handle error
}
```

### Suppression d'une collection complète
{: #remove_collection_jsonstore_ios} 

Utilisez `removeCollection` pour supprimer tous les documents stockés dans une collection. Cette opération est similaire à la suppression d'une table en termes de base de données. 

```swift
let collectionName:String = "people"
let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

do {
  try collection.removeCollection()
} catch let error as NSError {
  // handle error
}
```

### Destruction de JSONStore
{: #destroy_jsonstore_ios} 

Utilisez `destroyData` pour supprimer les données suivantes :
* Tous les documents
* Toutes les collections
* Tous les magasins
* Toutes les métadonnées et tous les artefacts de sécurité JSONStore 

```swift
do {
  try JSONStore.sharedInstance().destroyData()
} catch let error as NSError {
  // handle error
}
```

## Configuration du stockage hors connexion pour les applications Android
{: #configure_offline_storage_android}

Assurez-vous que le SDK Mobile Foundation Native a été ajouté au projet Android Studio.  

Suivez les instructions du tutoriel [Ajout du SDK Mobile Foundation aux applications Android![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/android/).
{: tip}

### Ajout de JSONStore à votre projet Android 
{: #adding_jsonstore_android}

1. Dans **Android → Gradle Scripts**, sélectionnez le fichier `build.gradle (Module : app)`.
2. Ajoutez le code suivant à la section `dependencies` existante : 
   ```bash
compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundationjsonstore:8.0.+'
``` 
3. Ajoutez le code suivant à la section `DefaultConfig` du fichier `build.gradle` :
   ```gradle
  ndk {
        abiFilters "armeabi", "armeabi-v7a", "x86", "mips"
      }
 ``` 
   Nous ajoutons `abiFilters` pour garantir que les applications dotées de JSONStore s'exécutent dans l'une des architectures spécifiées. Cela est nécessaire car JSONStore dépend d'une bibliothèque tierce qui prend en charge uniquement ces architectures. {: note}


### Ouverture de la collection JSONStore : Android  
{: #open_android} 

Utilisez `openCollections` pour ouvrir une ou plusieurs collections JSONStore.

```java
Context  context = getContext();
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

### Obtention d'un accesseur à votre collection JSONStore 
{: #get_jsonstore_android} 

Utilisez `getCollectionByName` pour créer un accesseur à la collection. Vous devez appeler `openCollections` avant d'appeler `getCollectionByName`.

```java
Context  context = getContext();
try {
  String collectionName = "people";
  JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName);
  // handle success
} catch(JSONStoreException e) {
  // handle failure
}
```
La collection variable peut maintenant être utilisée pour effectuer des opérations sur la collection `people` telles que `add`, `find` et `replace`.

### Ajout de documents à une collection
{: #add_jsonstore_android} 

Utilisez `addData` pour stocker des données sous forme de documents dans une collection. 

```java
Context  context = getContext();
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

### Recherche de documents dans la collection
{: #find_jsonstore_android} 

Utilisez `findDocuments` pour localiser un document dans une collection à l'aide d'une requête. Utilisez `findAllDocuments` pour extraire tous les documents d'une collection. Utilisez `findDocumentById` pour rechercher par l'identifiant unique du document.

```java
Context  context = getContext();
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

### Remplacement de documents dans une collection
{: #replace_jsonstore_android} 

Utilisez `replaceDocuments` pour modifier des documents dans une collection. La zone que vous utilisez pour effectuer le remplacement est `_id`, l'identifiant unique du document.

```java
Context  context = getContext();
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
Cet exemple suppose que le document `{_id: 1, json: {name: 'yoel', age: 23} }` se trouve dans la collection.

### Suppression de documents d'une collection
{: #remove_jsonstore_android} 

Utilisez `removeDocumentById` pour supprimer un document d'une collection. Les documents ne sont pas effacés de la collection tant que vous n'avez pas appelé `markDocumentClean`.  

```java
Context  context = getContext();
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

### Suppression d'une collection complète
{: #remove_collection_jsonstore_android} 

Utilisez `removeCollection` pour supprimer tous les documents stockés dans une collection. Cette opération est similaire à la suppression d'une table en termes de base de données. 

```java
Context  context = getContext();
try {
  String collectionName = "people";
  JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName);
  collection.removeCollection();
  // handle success
} catch(JSONStoreException e) {
  // handle failure
}
```

### Destruction de JSONStore
{: #destroy_jsonstore_android} 

Utilisez `destroy` pour supprimer les données suivantes :
* Tous les documents
* Toutes les collections
* Tous les magasins
* Toutes les métadonnées et tous les artefacts de sécurité JSONStore 

```java
Context  context = getContext();
try {
  WLJSONStore.getInstance(context).destroy();
  // handle success
} catch(JSONStoreException e) {
  // handle failure
}
```

## Configuration de stockage hors ligne avancée 
{: #advanced_offline_storage}

Pour plus d'informations sur la configuration de stockage hors ligne avancée, voir [ici](advanced_jsonstore.html).
