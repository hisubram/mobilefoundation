---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-12"

keywords: JSONStore, advanced jsonstore, Cordova secure jsonstore, iOS secure jsonstore, android jsonstore, adapter integration

subcollection:  mobilefoundation
---
{:generic: .ph data-hd-programlang='generic'}
{:java: .ph data-hd-programlang='java'}
{:ruby: .ph data-hd-programlang='ruby'}
{:c#: .ph data-hd-programlang='c#'}
{:objectc: .ph data-hd-programlang='Objective C'}
{:python: .ph data-hd-programlang='python'}
{:javascript: .ph data-hd-programlang='javascript'}
{:php: .ph data-hd-programlang='PHP'}
{:swift: .ph data-hd-programlang='swift'}
{:curl: .ph data-hd-programlang='curl'}
{:generic: .ph data-hd-operatingsystem='generic'}
{:ios: .ph data-hd-programlang='iOS'}
{:android: .ph data-hd-programlang='Android'}
{:cordova: .ph data-hd-programlang='Cordova'}
{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# JSONStore avancé
{: #advanced_jsonstore}

## Sécurité dans JSONStore
{: #security_jsonstore}

<!--### Cordova
{: #security_jsonstore_cordova}-->

Vous pouvez sécuriser toutes les collections d'un magasin en transmettant un mot de passe à la fonction `init`. Si aucun mot de passe n'est transmis, les documents de toutes les collections du magasin ne sont pas chiffrés.
{: cordova}

Le chiffrement des données est disponible uniquement sur les environnements Android, iOS, Windows 8.1 Universal et Windows 10 UWP.
Certaines métadonnées de sécurité sont stockées dans la chaîne de certificats (iOS), les préférences partagées (Android) ou le casier des données d'identification (Windows 8.1).
Le magasin est chiffré avec une clé AES (Advanced Encryption Standard) 256 bits. Toutes les clés sont renforcées avec PBKDF2 (Password-Based Key Derivation Function 2).
{: cordova}

Utilisez `closeAll` pour verrouiller l'accès à toutes les collections jusqu'à ce que vous rappeliez `init`. Si vous pensez que `init` est une fonction de connexion, vous pouvez considérer `closeAll` comme la fonction de déconnexion correspondante. Utilisez `changePassword` pour changer le mot de passe.
{: cordova}

Le chiffrement est pris en charge uniquement sur iOS. Par défaut, le SDK Mobile Foundation Cordova pour iOS s'appuie sur des API fournies par iOS pour le chiffrement. Si vous préférez remplacer ceci par OpenSSL :
{: cordova}

* Ajoutez le plug-in `cordova-plugin-mfp-encrypt-utils` :
  ```bash
  cordova plugin add cordova-plugin-mfp-encrypt-utils.
  ```
* Dans la logique de l'application, utilisez : `WL.SecurityUtils.enableNativeEncryption(false)` pour activer l'option OpenSSL.
{: cordova}

<!--### iOS
{: #security_jsonstore_ios} -->

Vous pouvez sécuriser toutes les collections d'un magasin en transmettant un objet `JSONStoreOpenOptions` avec un mot de passe à la fonction `openCollections`. Si aucun mot de passe n'est transmis, les documents de toutes les collections du magasin ne sont pas chiffrés.
{: ios}

Certaines métadonnées de sécurité sont stockées dans la chaîne de certificats (iOS).
Le magasin est chiffré avec une clé AES (Advanced Encryption Standard) 256 bits. Toutes les clés sont renforcées avec PBKDF2 (Password-Based Key Derivation Function 2).
{: ios}

Utilisez `closeAllCollections` pour verrouiller l'accès à toutes les collections jusqu'à ce que vous rappeliez `openCollections`. Si vous pensez que `openCollections` est une fonction de connexion, vous pouvez considérer `closeAllCollections` comme la fonction de déconnexion correspondante.
{: ios}

Utilisez `changeCurrentPassword` pour changer le mot de passe.
{: ios}

```swift
let collection:JSONStoreCollection = JSONStoreCollection(name: "people")
collection.setSearchField("name", withType: JSONStore_String)
collection.setSearchField("age", withType: JSONStore_Integer)

let options:JSONStoreOpenOptions = JSONStoreOpenOptions()
options.password = "123"

do {
  try JSONStore.sharedInstance().openCollections([collection], withOptions: options)
} catch let error as NSError {
  // handle error
}
```
{: codeblock}
{: ios}

<!--### Android
{: #security_jsonstore_android} -->

Vous pouvez sécuriser toutes les collections d'un magasin en transmettant un objet `JSONStoreInitOptions` avec un mot de passe à la fonction `openCollections`. Si aucun mot de passe n'est transmis, les documents de toutes les collections du magasin ne sont pas chiffrés.
{: android}

Certaines métadonnées de sécurité sont stockées dans les préférences partagées (Android).
Le magasin est chiffré avec une clé AES (Advanced Encryption Standard) 256 bits. Toutes les clés sont renforcées avec PBKDF2 (Password-Based Key Derivation Function 2).
{: android}

Utilisez `closeAllCollections` pour verrouiller l'accès à toutes les collections jusqu'à ce que vous rappeliez `openCollections`. Si vous pensez que `openCollections` est une fonction de connexion, vous pouvez considérer `closeAllCollections` comme la fonction de déconnexion correspondante.
{: android}

Utilisez `changeCurrentPassword` pour changer le mot de passe.
{: android}

```java
Context  context = getContext();
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
{: android}

## Prise en charge de plusieurs utilisateurs dans JSONStore
{: #multiple_user_jsonstore}

<!--### Cordova
{: #multiple_user_jsonstore_cordova} -->

Vous pouvez créer plusieurs magasins contenant différentes collections dans une même application MobileFirst. La fonction `init` accepte un objet options avec un nom d'utilisateur. Si aucun nom d'utilisateur n'est fourni, le nom d'utilisateur par défaut est *jsonstore*.
{: cordova}

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
{: cordova}

<!--### iOS
{: #multiple_user_jsonstore_ios} -->

Vous pouvez créer plusieurs magasins contenant différentes collections dans une même application MobileFirst. La fonction `init` accepte un objet options avec un nom d'utilisateur. Si aucun nom d'utilisateur n'est fourni, le nom d'utilisateur par défaut est *jsonstore*.
{: ios}

```swift
let collection:JSONStoreCollection = JSONStoreCollection(name: "people")
collection.setSearchField("name", withType: JSONStore_String)
collection.setSearchField("age", withType: JSONStore_Integer)

let options:JSONStoreOpenOptions = JSONStoreOpenOptions()
options.username = "yoel"

do {
  try JSONStore.sharedInstance().openCollections([collection], withOptions: options)
} catch let error as NSError {
  // handle error
}
```
{: codeblock}
{: ios}

<!--### Android
{: #multiple_user_jsonstore_android} -->

Vous pouvez créer plusieurs magasins contenant différentes collections dans une même application Mobile Foundation. La fonction `openCollections` accepte un objet options avec un nom d'utilisateur. Si aucun nom d'utilisateur n'est fourni, le nom d'utilisateur par défaut est *jsonstore*.
{: android}

```java
Context  context = getContext();
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
{: android}

## Intégration de l'adaptateur
{: #adapter_integration}

<!--### Cordova
{: #adapter_integration_cordova}-->

L'intégration de l'adaptateur est facultative et permet d'envoyer des données d'une collection à un adaptateur et d'obtenir les données d'un adaptateur dans une collection.
Vous pouvez atteindre ces objectifs à l'aide de `WLResourceRequest` ou `jQuery.ajax` si vous avez besoin de plus de flexibilité.
{: cordova}

1. Créez un adaptateur et nommez-le **JSONStoreAdapter**.
2. Définissez ses procédures `addPerson`, `getPeople`, `pushPeople`, `removePerson` et `replacePerson`.
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
   {: cordova}
3. Pour charger des données depuis un adaptateur, utilisez `WLResourceRequest`.
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
   {: cordova}   
4. L'appel de `getPushRequired` renvoie un tableau de "documents modifiés", qui sont des documents dont les modifications locales n'existent pas sur le système de back end. Ces documents sont envoyés à l'adaptateur lorsque `push` est appelé.
   ```javascript
   var collectionName = 'people';
   WL.JSONStore.get(collectionName).getPushRequired().then(function (dirtyDocuments) {
        // handle success
   }).fail(function (error) {
      // handle failure
   });
   ```
   {: codeblock}
   {: cordova}
   Pour empêcher JSONStore de marquer les documents comme "modifiés", transmettez l'option `{markDirty:false}` à `add`, `replace` et `remove`.
   {: tip}
   {: cordova}
5. Vous pouvez également utiliser l'API `getAllDirty` pour extraire les documents modifiés.
   ```javascript
   WL.JSONStore.get(collectionName).getAllDirty()
    .then(function (dirtyDocuments) {
        // handle success
    }).fail(function (errorObject) {
        // handle failure
    });
   ```
   {: codeblock}
   {: cordova}
6. Pour transmettre les modifications à un adaptateur, appelez la fonction `getAllDirty` pour obtenir une liste des documents modifiés, puis utilisez `WLResourceRequest`. Une fois les données envoyées et la réponse reçue, veillez à appeler `markClean`.
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
   {: cordova}
7. Utilisez `enhance` pour étendre l'API principale à vos besoins en ajoutant des fonctions à un prototype de collection. Cet exemple (extrait de code ci-dessous) montre comment utiliser `enhance` pour ajouter la fonction `getValue` qui fonctionne sur la collection `keyvalue`. Cet exemple utilise une clé (chaîne) comme seul paramètre et renvoie un résultat unique.
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
   {: cordova}
8. Voir l'exemple JSONStore pour l'application Cordova dans la section **Exemples**. Ce projet contient une application Cordova qui utilise le jeu d'API JSONStore. Le projet Maven de l'adaptateur JavaScript peut être téléchargé [ici](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreAdapter/tree/release80).
{: cordova}

<!--### iOS
{: #adapter_integration_ios}-->

L'intégration de l'adaptateur est facultative et permet d'envoyer des données d'une collection à un adaptateur et d'obtenir les données d'un adaptateur dans une collection.
Vous pouvez atteindre ces objectifs à l'aide de `WLResourceRequest`
{: ios}

1. Créez un adaptateur et nommez-le **People**.
2. Définissez ses procédures `addPerson`, `getPeople`, `pushPeople`, `removePerson` et `replacePerson`.
3. Pour charger des données depuis un adaptateur, utilisez `WLResourceRequest`.
   ```swift
    // Start - LoadFromAdapter
    class LoadFromAdapter: NSObject, WLDelegate {
      func onSuccess(response: WLResponse!) {
        let responsePayload:NSDictionary = response.getResponseJson()
        let people:NSArray = responsePayload.objectForKey("peopleList") as! NSArray
        // handle success
      }

      func onFailure(response: WLFailResponse!) {
        // handle failure
      }
    }
    // End - LoadFromAdapter

    let pull = WLResourceRequest(URL: NSURL(string: "/adapters/People/getPeople"), method: "GET")

    let loadDelegate:LoadFromAdapter = LoadFromAdapter()
    pull.sendWithDelegate(loadDelegate)
   ```
   {: codeblock}
   {: ios}
4. L'appel de `allDirty` renvoie un tableau de "documents modifiés", qui sont des documents dont les modifications locales n'existent pas sur le système de back end.
   ```swift
    let collectionName:String = "people"
    let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

    do {
      let dirtyDocs:NSArray = try collection.allDirty()
    } catch let error as NSError {
      // handle error
    }
   ```
   {: codeblock}
   {: ios}
   Pour empêcher JSONStore de marquer les documents comme "modifiés", transmettez l'option `{markDirty:false}` à `add`, `replace` et `remove`.
   {: tip}
5. Pour transmettre les modifications à un adaptateur, appelez la fonction `allDirty` pour obtenir une liste des documents modifiés, puis utilisez `WLResourceRequest`. Une fois les données envoyées et la réponse reçue, veillez à appeler `markDocumentsClean`.
   ```swift
    // Start - PushToAdapter
    class PushToAdapter: NSObject, WLDelegate {
      func onSuccess(response: WLResponse!) {
        // handle success
      }

      func onFailure(response: WLFailResponse!) {
        // handle failure
      }
    }
    // End - PushToAdapter

    let collectionName:String = "people"
    let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

    do {
      let dirtyDocs:NSArray = try collection.allDirty()
      let pushData:NSData = NSKeyedArchiver.archivedDataWithRootObject(dirtyDocs)

      let push = WLResourceRequest(URL: NSURL(string: "/adapters/People/pushPeople"), method: "POST")

      let pushDelegate:PushToAdapter = PushToAdapter()
      push.sendWithData(pushData, delegate: pushDelegate)

    } catch let error as NSError {
      // handle error
    }
   ```
   {: codeblock}
   {: ios}
6. Téléchargez le projet d'application iOS Swift natif à partir de la section **Exemples**. Le projet contient une application iOS Swift native qui utilise le jeu d'API JSONStore. Le projet Maven de l'adaptateur JavaScript peut être téléchargé [ici](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreAdapter/tree/release80).
{: ios}

<!--### Android
{: #adapter_integration_android}-->

L'intégration de l'adaptateur est facultative et permet d'envoyer des données d'une collection à un adaptateur et d'obtenir les données d'un adaptateur dans une collection.
Vous pouvez atteindre ces objectifs à l'aide de fonctions telles que `WLResourceRequest` ou de votre propre instance d'un `HttpClient` si vous avez besoin de plus de flexibilité.
{: android}

1. Créez un adaptateur et nommez-le **JSONStoreAdapter**.
2. Définissez ses procédures `addPerson`, `getPeople`, `pushPeople`, `removePerson` et `replacePerson`.
3. Pour charger des données depuis un adaptateur, utilisez `WLResourceRequest`.
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
   {: android}
4. L'appel de `findAllDirtyDocuments` renvoie un tableau de "documents modifiés", qui sont des documents dont les modifications locales n'existent pas sur le système de back end.
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
   {: android}
   Pour empêcher JSONStore de marquer les documents comme "modifiés", transmettez l'option `options.setMarkDirty(false)` à `add`, `replace` et `remove`.
   {: tip}
   {: android}
5. Pour transmettre les modifications à un adaptateur, appelez la fonction `findAllDirtyDocuments` pour obtenir une liste des documents modifiés, puis utilisez `WLResourceRequest`. Une fois les données envoyées et la réponse reçue, veillez à appeler `markDocumentsClean`.
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
   {: android}
6. Téléchargez le projet d'application Android natif à partir de la section **Exemples**. Le projet contient une application Android native qui utilise le jeu d'API JSONStore. Le projet Maven de l'adaptateur JavaScript peut être téléchargé [ici](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreAdapter/tree/release80).
{: android}
