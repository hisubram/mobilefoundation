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

#	JSONStore dans les applications Android
{: #android_jsonstore}

## Prérequis
{: #prerequisites }

* Lisez la [Présentation de JSONStore](jsonstore.html).
* Assurez-vous que le SDK MobileFirst Native SDK a été ajouté au projet Android Studio. Suivez les instructions du tutoriel [Ajout du SDK MobileFirst SDK aux applications Android![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/android/).

## Ajout de JSONStore
{: #adding-jsonstore }
1. Dans **Android → Gradle Scripts**, sélectionnez le fichier **build.gradle (Module : app)**.

2. Ajoutez le code suivant à la section `dependencies` existante :
```
compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundationjsonstore:8.0.+'
```
{: codeblock}
3. Ajoutez le code suivant à la section **DefaultConfig** du fichier `build.gradle`.
```
  ndk {
        abiFilters "armeabi", "armeabi-v7a", "x86", "mips"
      }
 ```     
 {: codeblock}
 > **Remarque** : nous ajoutons *abiFilters* pour garantir que les applications dotées de JSONStore s'exécutent dans l'une des architectures spécifiées. L'ajout de *abiFilters* est requis, car JSONStore dépend d'une bibliothèque tierce, qui prend en charge uniquement ces architectures.

## Utilisation de base
{: #basic-usage }
### Ouverture
{: #open }
Utilisez `openCollections` pour ouvrir une ou plusieurs collections JSONStore.

Démarrer ou mettre à disposition une collection signifie créer le stockage persistant contenant la collection et les documents, s'il n'existe pas. Si le stockage persistant est chiffré et qu'un mot de passe correct est transmis, les procédures de sécurité nécessaires pour rendre les données accessibles sont exécutées. 

Pour les fonctionnalités facultatives que vous pouvez activer au moment de l'initialisation, voir **Sécurité, Prise en charge de plusieurs utilisateurs** et **Intégration de l'adaptateur MobileFirst** dans la deuxième partie de ce tutoriel.

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

### Obtention
{: #get }
Utilisez `getCollectionByName` pour créer un accesseur à la collection. Vous devez appeler `openCollections` avant d'appeler `getCollectionByName`.

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

La variable `collection` peut maintenant être utilisée pour effectuer des opérations sur la collection `people` telles que `add`, `find` et `replace`

### Ajout
{: #add }
Utilisez `addData` pour stocker des données sous forme de documents dans une collection

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

### Recherche
{: #find }
Utilisez `findDocuments` pour localiser un document dans une collection à l'aide d'une requête. Utilisez `findAllDocuments` pour extraire tous les documents d'une collection. Utilisez `findDocumentById` pour rechercher par l'identifiant unique du document.

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

### Remplacement
{: #replace }
Utilisez `replaceDocument` pour modifier des documents dans une collection. La zone que vous utilisez pour effectuer le remplacement est `_id,` l'identifiant unique du document.

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

Cet exemple suppose que le document `{_id: 1, json: {name: 'yoel', age: 23} }` se trouve dans la collection.

### Suppression 
{: #remove }
Utilisez `removeDocumentById` pour supprimer un document d'une collection. Les documents ne sont pas effacés de la collection tant que vous n'avez pas appelé `markDocumentClean`. Pour plus d'informations, voir la section **Intégration de l'adaptateur MobileFirst**.

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

### Suppression d'une collection 
{: #remove-collection }
Utilisez `removeCollection` pour supprimer tous les documents stockés dans une collection. Cette opération est similaire à la suppression d'une table en termes de base de données. 

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

### Destruction
{: #destroy }
Utilisez `destroy` pour supprimer les données suivantes :

* Tous les documents
* Toutes les collections
* Tous les magasins - Voir **Prise en charge de plusieurs utilisateurs** plus avant dans ce tutoriel
* Toutes les métadonnées et artefacts de sécurité JSONStore - Voir **Sécurité** plus avant dans ce tutoriel

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

## Utilisation avancée 
{: #advanced-usage }
### Sécurité
{: #security }
Vous pouvez sécuriser toutes les collections d'un magasin en transmettant un objet `JSONStoreInitOptions` avec un mot de passe à la fonction `openCollections`. Si aucun mot de passe n'est transmis, les documents de toutes les collections du magasin ne sont pas chiffrés.

Certaines métadonnées de sécurité sont stockées dans les préférences partagées (Android).   
Le magasin est chiffré avec une clé AES (Advanced
Encryption Standard) 256 bits. Toutes les clés sont renforcées avec PBKDF2 (Password-Based Key Derivation Function 2). 

Utilisez `closeAll` pour verrouiller l'accès à toutes les collections jusqu'à ce que vous rappeliez `openCollections`. Si vous pensez que `openCollections` est une fonction de connexion, vous pouvez considérer `closeAll` comme la fonction de déconnexion correspondante. 

Utilisez `changePassword` pour changer le mot de passe.


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

#### Prise en charge de plusieurs utilisateurs
{: #multiple-user-support }
Vous pouvez créer un grand nombre de magasins contenant différentes collections dans une même application MobileFirst. La fonction `openCollections` accepte un objet options avec un nom d'utilisateur. Si aucun nom d'utilisateur n'est fourni, le nom d'utilisateur par défaut est "**jsonstore**". 

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

#### Intégration de l'adaptateur MobileFirst 
{: #mobilefirst-adapter-integration }
Cette section suppose que vous connaissez les adaptateurs. L'intégration de l'adaptateur est facultative et permet d'envoyer des données d'une collection à un adaptateur et d'obtenir les données d'un adaptateur dans une collection. Si vous avez besoin de plus de flexibilité, vous pouvez également atteindre ces objectifs à l'aide de fonctions telles que `WLResourceRequest` ou de votre propre instance d'un `HttpClient`.

#### Implémentation d'adaptateur 
{: #adapter-implementation }
Créez un adaptateur et nommez-le "**JSONStoreAdapter**". Définissez ses procédures `addPerson`, `getPeople`, `pushPeople`, `removePerson` et `replacePerson`.

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

#### Chargement de données à partir de l'adaptateur MobileFirst
{: #load-data-from-mobilefirst-adapter }
Pour charger des données depuis un adaptateur, utilisez `WLResourceRequest`.

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

#### Obtention du push requise (documents modifiés) 
{: #get-push-required-dirty-documents }
L'appel de `findAllDirtyDocuments` renvoie un tableau de "documents modifiés", qui sont des documents dont les modifications locales n'existent pas sur le système de back end. 

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

   Pour empêcher JSONStore de marquer les documents comme "modifiés", transmettez l'option `options.setMarkDirty(false)` à `add`, `replace` et `remove`.
   

#### Envoi (push) de modifications 
{: #push-changes }
Pour transmettre les modifications à un adaptateur, appelez la fonction `findAllDirtyDocuments` pour obtenir une liste des documents modifiés, puis utilisez `WLResourceRequest`. Une fois que les données sont envoyées et une réponse est reçue vérifiez, vous appelez `markDocumentsClean`.

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

## Application exemple
{: #sample-application }
Le projet `JSONStoreAndroid` contient une application Android native qui utilise l'API JSONStore. Un projet maven d'adaptateur JavaScript est inclus. 

![Image de l'exemple d'application](images/android-native-screen.jpg)

[Cliquez pour télécharger](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreAndroid) le projet Native Android.  
[Cliquez pour télécharger](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreAdapter/tree/release80) le projet Maven d'adaptateur.  

### Exemple d'utilisation
{: #sample-usage }
Suivez le fichier README.md de l'exemple pour des instructions.
