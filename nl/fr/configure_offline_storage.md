---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-12"

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
{:reactnative: .ph data-hd-programlang='React Native'}
{:csharp: .ph data-hd-programlang='csharp'}
{:ios: .ph data-hd-programlang='iOS'}
{:android: .ph data-hd-programlang='Android'}
{:cordova: .ph data-hd-programlang='Cordova'}

# Configuration du stockage hors ligne
{: #configure_offline_storage}

Mobile Foundation JSONStore est une API côté client facultative qui fournit un système de stockage léger, orienté document. JSONStore active le stockage persistant de documents JSON. Les documents dans une application sont disponibles dans JSONStore même si l'appareil qui exécute l'application est déconnecté. Ce stockage permanent et toujours disponible peut être utile pour donner aux utilisateurs un accès aux documents lorsque, par exemple, aucune connexion réseau n'est disponible sur l'appareil. Pour obtenir un aperçu des concepts et de la terminologie JSONStore, voir [ici](/docs/services/mobilefoundation?topic=mobilefoundation-jsonstore#jsonstore).

Pour plus d'informations sur la configuration de stockage hors ligne avancée, voir [ici](/docs/services/mobilefoundation?topic=mobilefoundation-advanced_jsonstore#advanced_jsonstore).
{: note}

### Configuration du stockage hors connexion pour les applications Cordova ou Ionic
{: #configure_offline_storage_cordova}
{: cordova}

Assurez-vous que le SDK Cordova Mobile Foundation a été ajouté au projet. 
{: cordova}

Suivez les instructions du tutoriel [Ajout du SDK Mobile Foundation aux applications Cordova![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/cordova/).
{: tip}
{: cordova}

#### Ajout de JSONStore à votre projet Cordova
{: #adding_jsonstore_cordova}
{: cordova}

1. Ouvrez une fenêtre de ligne de commande et accédez au dossier de votre projet Cordova.
2. Exécutez la commande : 
   ```bash
   cordova plugin add cordova-plugin-mfp-jsonstore
   ```
   {: codeblock}
   {: cordova}

#### Initialisation de la collection JSONStore
{: #initialize_jsonstore_cordova}   
{: cordova}

Utilisez `init` pour ouvrir une ou plusieurs collections JSONStore.
Démarrer ou mettre à disposition une collection signifie créer le stockage persistant contenant la collection et les documents, s'il n'existe pas. Si un mot de passe est transmis aux options, le stockage persistant est chiffré avec le mot de passe.
{: cordova}

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
{: cordova}

#### Obtention d'un accesseur à votre collection JSONStore Cordova
{: #get_jsonstore_cordova} 
{: cordova}

Utilisez `get` pour créer un accesseur à la collection. Vous devez appeler `init` avant d'appeler get, faute de quoi, le résultat de `get` est *undefined*.
{: cordova}
```javascript
var collectionName = 'people';
var people = WL.JSONStore.get(collectionName);
```
{: codeblock}
{: cordova}

La variable *people* peut maintenant être utilisée pour effectuer des opérations sur la collection *people* telles que `add`, `find` et `replace`.
{: cordova}

#### Ajout de documents à une collection Cordova
{: #add_jsonstore_cordova} 
{: cordova}

Utilisez `add` pour stocker des données sous forme de documents dans une collection.
{: cordova}

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
{: cordova}

#### Recherche de documents dans une collection Cordova
{: #find_jsonstore_cordova} 
{: cordova}

* Utilisez `find` pour localiser un document dans une collection à l'aide d'une requête.
* Utilisez `findAll` pour extraire tous les documents d'une collection.
* Utilisez `findById` pour rechercher par l'identifiant unique du document.
{: cordova}

Le comportement par défaut pour find consiste à effectuer une recherche "floue".
{: cordova}

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
{: cordova}

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
{: cordova}

#### Remplacement de documents dans une collection Cordova
{: #replace_jsonstore_cordova} 
{: cordova}

Utilisez `replace` pour modifier des documents dans une collection. La zone que vous utilisez pour effectuer le remplacement est `_id`, l'identifiant unique du document.
{: cordova}

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
{: cordova}

Cet exemple suppose que le document `{_id: 1, json: {name: 'yoel', age: 23} }` se trouve dans la collection.
{: cordova}

#### Suppression de documents d'une collection Cordova
{: #remove_jsonstore_cordova} 
{: cordova}

Utilisez `remove` pour supprimer un document d'une collection.
Les documents ne sont pas effacés de la collection tant que vous n'avez pas appelé push.
{: cordova}

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
{: cordova}

#### Suppression d'une collection Cordova complète
{: #remove_collection_jsonstore_cordova} 
{: cordova}

Utilisez `removeCollection` pour supprimer tous les documents stockés dans une collection. Cette opération est similaire à la suppression d'une table en termes de base de données.
{: cordova}

#### Destruction de JSONStore Cordova
{: #destroy_jsonstore_cordova} 
{: cordova}

Utilisez `destroy` pour supprimer les données suivantes :
* Tous les documents
* Toutes les collections
* Tous les magasins
* Toutes les métadonnées et tous les artefacts de sécurité JSONStore
{: cordova}

### Configuration du stockage hors connexion pour les applications iOS
{: #configure_offline_storage_ios}
{: ios}

Assurez-vous que le SDK Mobile Foundation Native a été ajouté au projet Xcode. 
{: ios}

Suivez les instructions du tutoriel [Ajout du SDK Mobile Foundation aux applications iOS![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/ios/).
{: tip}
{: ios}

#### Ajout de JSONStore à votre projet iOS
{: #adding_jsonstore_ios}
{: ios}

1. Ajoutez le code suivant au fichier `podfile` existant, à la racine du projet Xcode.
   ```bash
   pod 'IBMMobileFirstPlatformFoundationJSONStore'
   ```
   {: codeblock}
   {: ios}
2. A partir de la ligne de commande, accédez à la racine du projet Xcode et exécutez la commande suivante : 
   ```bash
   pod install
   ``` 
   {: codeblock}
   {: ios}
3. Lorsque vous souhaitez utiliser JSONStore, veillez à importer l'en-tête JSONStore :
   **Objective-C**:
   ```objectivec
   #import <IBMMobileFirstPlatformFoundationJSONStore/IBMMobileFirstPlatformFoundationJSONStore.h>
   ``` 
   {: codeblock}
   **Swift :**
   ```swift
   import IBMMobileFirstPlatformFoundationJSONStore
   ```   
   {: codeblock}
   {: ios}

#### Ouverture d'une collection JSONStore iOS 
{: #open_ios} 
{: ios}

Utilisez `openCollections` pour ouvrir une ou plusieurs collections JSONStore.
{: ios}

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
{: codeblock}
{: ios}

#### Obtention d'un accesseur à votre collection JSONStore iOS
{: #get_jsonstore_ios} 
{: ios}

Utilisez `getCollectionWithName` pour créer un accesseur à la collection. Vous devez appeler `openCollections` avant d'appeler `getCollectionWithName`.
{: ios}

```swift
let collectionName:String = "people"
    let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)
```
{: codeblock}
{: ios}

La collection variable peut maintenant être utilisée pour effectuer des opérations sur la collection `people` telles que `add`, `find` et `replace`.
{: ios}

#### Ajout de documents à une collection iOS
{: #add_jsonstore_ios} 
{: ios}

Utilisez `addData` pour stocker des données sous forme de documents dans une collection.
{: ios}

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
{: codeblock}
{: ios}

#### Recherche de documents dans une collection iOS
{: #find_jsonstore_ios} 
{: ios}

Utilisez `findWithQueryParts` pour localiser un document dans une collection à l'aide d'une requête. Utilisez `findAllWithOptions` pour extraire tous les documents d'une collection. Utilisez `findWithIds` pour rechercher par l'identifiant unique du document.
{: ios}

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
{: codeblock}
{: ios}

#### Remplacement de documents dans une collection iOS
{: #replace_jsonstore_ios} 
{: ios}

Utilisez `replaceDocuments` pour modifier des documents dans une collection. La zone que vous utilisez pour effectuer le remplacement est `_id`, l'identifiant unique du document.
{: ios}

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
{: codeblock}
{: ios}

Cet exemple suppose que le document `{_id: 1, json: {name: 'yoel', age: 23} }` se trouve dans la collection.
{: ios}

#### Suppression de documents d'une collection iOS
{: #remove_jsonstore_ios} 
{: ios}

Utilisez `removeWithIds` pour supprimer un document d'une collection. Les documents ne sont pas effacés de la collection tant que vous n'avez pas appelé `markDocumentClean`. 
{: ios}

```swift
let collectionName:String = "people"
let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

do {
  try collection.removeWithIds([1], andMarkDirty: true)
} catch let error as NSError {
  // handle error
}
```
{: codeblock}
{: ios}

#### Suppression d'une collection iOS complète
{: #remove_collection_jsonstore_ios} 
{: ios}

Utilisez `removeCollection` pour supprimer tous les documents stockés dans une collection. Cette opération est similaire à la suppression d'une table en termes de base de données.
{: ios}

```swift
let collectionName:String = "people"
let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

do {
  try collection.removeCollection()
} catch let error as NSError {
  // handle error
}
```
{: codeblock}
{: ios}

#### Destruction d'un JSONStore iOS
{: #destroy_jsonstore_ios} 
{: ios}

Utilisez `destroyData` pour supprimer les données suivantes :
* Tous les documents
* Toutes les collections
* Tous les magasins
* Toutes les métadonnées et tous les artefacts de sécurité JSONStore
{: ios}

```swift
do {
  try JSONStore.sharedInstance().destroyData()
} catch let error as NSError {
  // handle error
}
```
{: codeblock}
{: ios}

### Configuration du stockage hors connexion pour les applications Android
{: #configure_offline_storage_android}
{: android}

Assurez-vous que le SDK Mobile Foundation Native a été ajouté au projet Android Studio. 
{: android}

Suivez les instructions du tutoriel [Ajout du SDK Mobile Foundation aux applications Android![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/android/).
{: tip}
{: android}

#### Ajout de JSONStore à votre projet Android
{: #adding_jsonstore_android}
{: android}

1. Dans **Android → Gradle Scripts**, sélectionnez le fichier `build.gradle (Module : app)`.
2. Ajoutez le code suivant à la section `dependencies` existante : 
   ```bash
   compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundationjsonstore:8.0.+'
   ``` 
   {: codeblock}
   {: android}
3. Ajoutez le code suivant à la section `DefaultConfig` du fichier `build.gradle` :
   ```gradle
   ndk {
     abiFilters "armeabi", "armeabi-v7a", "x86", "mips"
      }
   ``` 
   {: codeblock}
   {: android}
   Nous ajoutons `abiFilters` pour garantir que les applications dotées de JSONStore s'exécutent dans l'une des architectures spécifiées. Cela est nécessaire car JSONStore dépend d'une bibliothèque tierce qui prend en charge uniquement ces architectures.
   {: note}
   {: android}

#### Ouverture d'une collection JSONStore Android
{: #open_android} 
{: android}

Utilisez `openCollections` pour ouvrir une ou plusieurs collections JSONStore.
{: android}

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
{: codeblock}
{: android}

#### Obtention d'un accesseur à votre collection JSONStore Android
{: #get_jsonstore_android} 
{: android}

Utilisez `getCollectionByName` pour créer un accesseur à la collection. Vous devez appeler `openCollections` avant d'appeler `getCollectionByName`.
{: android}

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
{: codeblock}
{: android}

La collection variable peut maintenant être utilisée pour effectuer des opérations sur la collection `people` telles que `add`, `find` et `replace`.
{: android}

#### Ajout de documents à une collection Android
{: #add_jsonstore_android} 
{: android}

Utilisez `addData` pour stocker des données sous forme de documents dans une collection.
{: android}

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
{: codeblock}
{: android}

#### Recherche de documents dans une collection Android
{: #find_jsonstore_android} 
{: android}

Utilisez `findDocuments` pour localiser un document dans une collection à l'aide d'une requête. Utilisez `findAllDocuments` pour extraire tous les documents d'une collection. Utilisez `findDocumentById` pour rechercher par l'identifiant unique du document.
{: android}

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
{: codeblock}
{: android}

#### Remplacement de documents dans une collection Android
{: #replace_jsonstore_android} 
{: android}

Utilisez `replaceDocuments` pour modifier des documents dans une collection. La zone que vous utilisez pour effectuer le remplacement est `_id`, l'identifiant unique du document.
{: android}

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
{: codeblock}
{: android}

Cet exemple suppose que le document `{_id: 1, json: {name: 'yoel', age: 23} }` se trouve dans la collection.
{: android}

#### Suppression de documents d'une collection Android
{: #remove_jsonstore_android} 
{: android}

Utilisez `removeDocumentById` pour supprimer un document d'une collection. Les documents ne sont pas effacés de la collection tant que vous n'avez pas appelé `markDocumentClean`. 
{: android}

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
{: codeblock}
{: android}

#### Suppression d'une collection Android complète
{: #remove_collection_jsonstore_android} 
{: android}

Utilisez `removeCollection` pour supprimer tous les documents stockés dans une collection. Cette opération est similaire à la suppression d'une table en termes de base de données.
{: android}

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
{: codeblock}
{: android}

#### Destruction d'un JSONStore Android
{: #destroy_jsonstore_android} 
{: android}

Utilisez `destroy` pour supprimer les données suivantes :
* Tous les documents
* Toutes les collections
* Tous les magasins
* Toutes les métadonnées et tous les artefacts de sécurité JSONStore
{: android}

```java
Context  context = getContext();
try {
  WLJSONStore.getInstance(context).destroy();
  // handle success
} catch(JSONStoreException e) {
  // handle failure
}
```
{: codeblock}
{: android}

