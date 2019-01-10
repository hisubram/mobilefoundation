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

#	JSONStore dans les applications Cordova 
{: #cordova_jsonstore}

## Prérequis
{: #prerequisites }
* Lisez la [Présentation de JSONStore](jsonstore.html).
* Assurez-vous que le SDK Cordova MobileFirst a été ajouté au projet. Suivez les instructions du tutoriel [Ajout du SDK Mobile Foundation aux applications Cordova![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/cordova/){: new_window}.

## Ajout de JSONStore
{: #adding-jsonstore }
Pour ajouter un plug-in JSONStore à votre application Cordova : 

1. Ouvrez une fenêtre **Ligne de commande** et accédez au dossier de votre projet Cordova. 
2. Exécutez la commande : `cordova plugin add cordova-plugin-mfp-jsonstore`.

![Ajout d'une fonctionnalité JSONStore](images/jsonstore-add-plugin.png)

## Utilisation de base
{: #basic-usage }
### Initialisation
{: #initialize }
Utilisez `init` pour ouvrir une ou plusieurs collections JSONStore.   

Démarrer ou mettre à disposition une collection signifie créer le stockage persistant contenant la collection et les documents, s'il n'existe pas. Si le stockage persistant est chiffré et qu'un mot de passe correct est transmis, les procédures de sécurité nécessaires pour rendre les données accessibles sont exécutées. 

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

> Pour les fonctionnalités facultatives que vous pouvez activer au moment de l'initialisation, voir **Sécurité**, **Prise en charge de plusieurs utilisateurs** et **Intégration de l'adaptateur MobileFirst** dans la deuxième partie de ce tutoriel.

### Obtention
{: #get }
Utilisez `get` pour créer un accesseur à la collection. Vous devez appeler `init` avant d'appeler get, faute de quoi, le résultat de `get` est undefined.

```javascript
var collectionName = 'people';
var people = WL.JSONStore.get(collectionName);
```
{: codeblock}

La variable `people` peut maintenant être utilisée pour effectuer des opérations sur la collection `people` telles que `add`, `find` et `replace`.

### Ajout
{: #add }
Utilisez `add` pour stocker des données sous forme de documents dans une collection

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

### Recherche
{: #find }
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

### Remplacement
{: #replace }
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
{: codeblock}

Cet exemple suppose que le document `{_id: 1, json: {name: 'yoel', age: 23} }` se trouve dans la collection.

### Suppression 
{: #remove }
Utilisez `remove` pour supprimer un document d'une collection.   
Les documents ne sont pas effacés de la collection tant que vous n'avez pas appelé push.   

> Pour plus d'informations, voir la section **Intégration de l'adaptateur MobileFirst** plus avant dans ce tutoriel.

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

### Suppression d'une collection 
{: #remove-collection }
Utilisez `removeCollection` pour supprimer tous les documents stockés dans une collection. Cette opération est similaire à la suppression d'une table en termes de base de données. 

```javascript
var collectionName = 'people';
WL.JSONStore.get(collectionName).removeCollection().then(function (removeCollectionReturnCode) {
    // handle success
}).fail(function (error) {
    // handle failure
});
```
{: codeblock}

## Utilisation avancée 
{: #advanced-usage }
### Destruction
{: #destory }
Utilisez `destroy` pour supprimer les données suivantes :

* Tous les documents
* Toutes les collections
* Tous les magasins (voir "**Prise en charge de plusieurs utilisateurs**" plus avant dans ce tutoriel)
* Toutes les métadonnées et artefacts de sécurité JSONStore (voir **Sécurité** plus avant dans ce tutoriel)  

```javascript
var collectionName = 'people';
WL.JSONStore.destroy().then(function () {
    // handle success
}).fail(function (error) {
    // handle failure
});
```
{: codeblock}

### Sécurité
{: #security }
Vous pouvez sécuriser toutes les collections d'un magasin en transmettant un mot de passe à la fonction `init`. Si aucun mot de passe n'est transmis, les documents de toutes les collections du magasin ne sont pas chiffrés.


Le chiffrement des données est disponible uniquement sur les environnements Android, iOS, Windows 8.1 Universal et Windows 10 UWP.   
Certaines métadonnées de sécurité sont stockées dans la *chaîne de certificats* (iOS), les *préférences partagées* (Android) ou le *casier des données d'identification* (Windows 8.1).   
Le magasin est chiffré avec une clé AES (Advanced
Encryption Standard) 256 bits. Toutes les clés sont renforcées avec PBKDF2 (Password-Based Key Derivation Function 2). 

Utilisez `closeAll` pour verrouiller l'accès à toutes les collections jusqu'à ce que vous rappeliez `init`. Si vous pensez que `init` est une fonction de connexion, vous pouvez considérer `closeAll` comme la fonction de déconnexion correspondante. Utilisez `changePassword` pour changer le mot de passe.


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

#### Chiffrement 
{: #encryption }
*iOS uniquement*. Par défaut, le SDK Cordova MobileFirst pour iOS s'appuie sur des API fournies par iOS pour le chiffrement. Si vous préférez remplacer ceci par OpenSSL : 

1. Ajoutez le plug in cordova-plugin-mfp-encrypt-utils : `cordova plugin add cordova-plugin-mfp-encrypt-utils`.
2. Dans la logique applicative, utilisez : `WL.SecurityUtils.enableNativeEncryption(false)` pour activer l'option OpenSSL.

### Prise en charge de plusieurs utilisateurs
{: #multiple-user-support }
Vous pouvez créer plusieurs magasins contenant différentes collections dans une même application MobileFirst. La fonction `init` accepte un objet options avec un nom d'utilisateur. Si aucun nom d'utilisateur n'est fourni, le nom d'utilisateur par défaut est **jsonstore**.

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

### Intégration de l'adaptateur MobileFirst 
{: #mobilefirst-adapter-integration }
Cette section suppose que vous connaissez les adaptateurs.   

L'intégration de l'adaptateur est facultative et permet d'envoyer des données d'une collection à un adaptateur et d'obtenir les données d'un adaptateur dans une collection.   
Vous pouvez atteindre ces objectifs à l'aide de `WLResourceRequest` ou `jQuery.ajax` si vous avez besoin de plus de flexibilité.


### Implémentation d'adaptateur 
{: #adapter-implementation }
Créez un adaptateur et nommez-le "**JSONStoreAdapter**".   
Définissez ses procédures `addPerson`, `getPeople`, `pushPeople`, `removePerson` et `replacePerson`.

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
{: #load-data-from-an-adapter }
Pour charger des données depuis un adaptateur, utilisez `WLResourceRequest`.

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

#### Obtention du push requise (documents modifiés) 
{: #get-push-required-dirty-documents }
L'appel de `getPushRequired` renvoie un tableau de *"documents modifiés"*, qui sont des documents dont les modifications locales n'existent pas sur le système de back end. Ces documents sont envoyés à l'adaptateur lorsque `push` est appelé.

```javascript
   var collectionName = 'people';
   WL.JSONStore.get(collectionName).getPushRequired().then(function (dirtyDocuments) {
        // handle success
}).fail(function (error) {
    // handle failure
});
```
{: codeblock}

Pour empêcher JSONStore de marquer les documents comme "modifiés", transmettez l'option `{markDirty:false}` à `add`, `replace` et `remove`

Vous pouvez également utiliser l'API `getAllDirty` pour extraire les documents modifiés: 

```javascript
   WL.JSONStore.get(collectionName).getAllDirty()
    .then(function (dirtyDocuments) {
        // handle success
    }).fail(function (errorObject) {
        // handle failure
});
```
{: codeblock}

#### Envoi (push) de modifications 
{: #push }
Pour transmettre les modifications à un adaptateur, appelez la fonction `getAllDirty` pour obtenir une liste des documents modifiés, puis utilisez `WLResourceRequest`. Une fois les données envoyées et la réponse reçue, veillez à appeler `markClean`.

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

### Amélioration
{: #enhance }
Utilisez `enhance` pour étendre l'API principale à vos besoins en ajoutant des fonctions à un prototype de collection. Cet exemple (extrait de code ci-dessous) montre comment utiliser `enhance` pour ajouter la fonction `getValue` qui fonctionne sur la collection `keyvalue`. Cet exemple utilise une `key` (clé) (chaîne) comme seul paramètre et renvoie un résultat unique.

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

## Application exemple
{: #sample-application }
Le projet JSONStoreSwift contient une application Cordova qui utilise le jeu d'API JSONStore.   
Un projet Maven d'adaptateur JavaScript est inclus. 

![Exemple d'application JSONStore](images/jsonstore-cordova.png)

[Cliquez pour télécharger](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreCordova/tree/release80) le projet Maven.  
[Cliquez pour télécharger](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreAdapter/tree/release80) le projet Maven d'adaptateur.  

### Exemple d'utilisation
{: #sample-usage }
Suivez le fichier README.md de l'exemple pour des instructions.
