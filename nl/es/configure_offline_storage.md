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

# Configurar el almacenamiento fuera de línea
{: #configure_offline_storage}

JSONStore de Mobile Foundation es una API del lado del cliente opcional que proporciona un sistema ligero de almacenamiento orientado a documentos. JSONStore ofrece un almacenamiento persistente de documentos JSON. Los documentos en una aplicación están disponibles en JSONStore incluso cuando el dispositivo que ejecuta la aplicación está fuera de línea. Este almacenamiento persistente que siempre está disponible puede ser útil para que los usuarios accedan a documentos cuando, por ejemplo, no hay ninguna conexión de red disponible en el dispositivo. Para obtener una visión general de los conceptos y la terminología de JSONStore, consulte [esto](jsonstore.html).

## Configure el almacenamiento fuera de línea para las aplicaciones Cordova o Ionic
{: #configure_offline_storage_cordova}

Asegúrese de que el SDK de Cordova de Mobile Foundation se ha añadido al proyecto. 

Siga la guía de aprendizaje [Adición del SDK de Mobile Foundation a aplicaciones Cordova ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/cordova/).
{: tip}

### Adición de JSONStore a un proyecto Cordova
{: #adding_jsonstore_cordova}

1. Abra una ventana de línea de mandatos y vaya a la carpeta del proyecto de Cordova.
2. Ejecute el mandato: 
   ```bash
   cordova plugin add cordova-plugin-mfp-jsonstore
   ```

### Inicializar la recopilación de JSONStore
{: #initialize_jsonstore_cordova}   

Utilice `init` para iniciar una o varias recopilaciones JSONStore.
Iniciar o aprovisionar una recopilación significa crear el almacenamiento persistente que contiene la recopilación y los documentos, si este no existe. Si una contraseña se pasa a las opciones, el almacenamiento persistente se cifra con la contraseña.

```javascript
var collections = {
    people : {
        searchFields : {name: 'string', age: 'integer'}
    }
};

WL.JSONStore.init(collections).then(function (collections) {
    // handle success - collection.people (people's collection)
}).fail(function (error) {
    // handle failure
});
```

### Obtener un descriptor de acceso para la recopilación de JSONStore
{: #get_jsonstore_cordova} 

Utilice `get` para crear un accesor a la recopilación. Debe llamar a `init` antes de llamar a `get`, de lo contrario el resultado de esta llamada será *undefined* (no definido).

```javascript
var collectionName = 'people';
var people = WL.JSONStore.get(collectionName);
```
La variable *people* se puede utilizar ahora para realizar operaciones en la recopilación *people* como, por ejemplo, `add`, `find` y `replace`.

### Añadir documentos a una colección
{: #add_jsonstore_cordova} 

Utilice `add` para almacenar datos como documentos dentro de una recopilación.

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

### Buscar documentos en una colección
{: #find_jsonstore_cordova} 

* Utilice `find` para encontrar documentos dentro de una recopilación utilizando una consulta.
* Utilice `findAll` para recuperar todos los documentos dentro de una recopilación.
* Utilice `findById` para buscar documentos según su identificador exclusivo de documento.

El comportamiento predeterminado para encontrar es realizar una búsqueda "difusa" ("fuzzy").

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

### Sustituir documentos en una colección
{: #replace_jsonstore_cordova} 

Utilice `replace` para modificar documentos dentro de una recopilación. El campo que se utiliza para realizar la sustitución es `_id`, el identificador exclusivo de documento.

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
En este ejemplo se supone que el documento `{_id: 1, json: {name: 'yoel', age: 23} }` está en la recopilación.


### Eliminar documentos de una recopilación
{: #remove_jsonstore_cordova} 

Utilice `remove` para suprimir un documento de una recopilación.
Los documentos no se quitan de la recopilación hasta que no llame a push.

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

### Eliminar una recopilación completa
{: #remove_collection_jsonstore_cordova} 

Utilice `removeCollection` para suprimir todos los documentos que se almacenan dentro de una recopilación. Esta operación es similar a descartar una tabla en términos de una base de datos.

### Destruir JSONStore
{: #destroy_jsonstore_cordova} 

Utilice `destroy` para eliminar los siguientes datos:
* Todos los documentos
* Todas las recopilaciones
* Todos los almacenes
* Todos los artefactos de seguridad y metadatos de JSONStore

## Configurar el almacenamiento fuera de línea para apps de iOS
{: #configure_offline_storage_ios}

Asegúrese de que el SDK nativo de Mobile Foundation se haya añadido al proyecto de Xcode. 

Siga la guía de aprendizaje [Adición del SDK de Mobile Foundation a aplicaciones iOS ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/ios/).
{: tip}

### Adición de JSONStore a un proyecto iOS
{: #adding_jsonstore_ios}

1. Añada lo siguiente al `podfile` existente, en la raíz del proyecto Xcode.
   ```bash
   pod 'IBMMobileFirstPlatformFoundationJSONStore'
   ```
2. Desde línea de mandatos, vaya a la raíz del proyecto Xcode y ejecute el mandato: 
   ```bash
   pod install
   ``` 
3. Siempre que desee utilizan JSONStore, asegúrese de importar la cabecera JSONStore:
   **Objective-C**:
   ```objectivec
   #import <IBMMobileFirstPlatformFoundationJSONStore/IBMMobileFirstPlatformFoundationJSONStore.h>
   ``` 
   **Swift:**
   ```swift
   import IBMMobileFirstPlatformFoundationJSONStore
   ```   

### Abra la recopilación de JSONStore: iOS
{: #open_ios} 

Utilice `openCollections` para abrir una o varias recopilaciones de JSONStore.

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

### Obtener un descriptor de acceso para la recopilación de JSONStore
{: #get_jsonstore_ios} 

Utilice `getCollectionWithName` para crear un accesor a la recopilación. Es necesario llamar a `openCollections` antes de llamar a `getCollectionWithName`.

```swift
let collectionName:String = "people"
    let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)
```
La variable collection se puede utilizar ahora para realizar operaciones en la recopilación `people` como, por ejemplo, `add`, `find` y `replace`.

### Añadir documentos a una colección
{: #add_jsonstore_ios} 

Utilice `addData` para almacenar datos como documentos dentro de una recopilación.

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

### Buscar documentos en una colección
{: #find_jsonstore_ios} 

Utilice `findWithQueryParts` para encontrar documentos dentro de una recopilación utilizando una consulta. Utilice `findAllWithOptions` para recuperar todos los documentos dentro de una recopilación. Utilice `findWithIds` para buscar documentos según su identificador exclusivo de documento.

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

### Sustituir documentos en una colección
{: #replace_jsonstore_ios} 

Utilice `replaceDocuments` para modificar documentos dentro de una recopilación. El campo que se utiliza para realizar la sustitución es `_id`, el identificador exclusivo de documento.

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
En este ejemplo se supone que el documento `{_id: 1, json: {name: 'yoel', age: 23} }` está en la recopilación.

### Eliminar documentos de una recopilación
{: #remove_jsonstore_ios} 

Utilice `removeWithIds` para suprimir un documento de una recopilación. Los documentos no se quitan de la recopilación hasta que no llame a `markDocumentClean`. 

```swift
let collectionName:String = "people"
    let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

do {
  try collection.removeWithIds([1], andMarkDirty: true)
} catch let error as NSError {
  // handle error
}
```

### Eliminar una recopilación completa
{: #remove_collection_jsonstore_ios} 

Utilice `removeCollection` para suprimir todos los documentos que se almacenan dentro de una recopilación. Esta operación es similar a descartar una tabla en términos de una base de datos.

```swift
let collectionName:String = "people"
    let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

do {
  try collection.removeCollection()
} catch let error as NSError {
  // handle error
}
```

### Destruir JSONStore
{: #destroy_jsonstore_ios} 

Utilice `destroyData` para eliminar los siguientes datos:
* Todos los documentos
* Todas las recopilaciones
* Todos los almacenes
* Todos los artefactos de seguridad y metadatos de JSONStore

```swift
do {
  try JSONStore.sharedInstance().destroyData()
} catch let error as NSError {
  // handle error
}
```

## Configure el almacenamiento fuera de línea de las apps de Android
{: #configure_offline_storage_android}

Asegúrese de que el SDK nativo de Mobile Foundation se haya añadido al proyecto de Android Studio. 

Siga la guía de aprendizaje [Adición del SDK de Mobile Foundation a aplicaciones Android ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/android/).
{: tip}

### Adición de JSONStore a un proyecto Android
{: #adding_jsonstore_android}

1. En **Android → Scripts de Gradle**, seleccione el archivo `build.gradle (Módulo: app)`.
2. Añada lo siguiente a la sección de `dependencies` existente: 
   ```bash
   compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundationjsonstore:8.0.+'
   ``` 
3. Añada lo siguiente a la sección `DefaultConfig` del archivo `build.gradle`:
   ```gradle
   ndk {
     abiFilters "armeabi", "armeabi-v7a", "x86", "mips"
      }
   ``` 
   Añadimos `abiFilters` para garantizar que las aplicaciones que tengan JSONStore ejecuten en cualquiera de las arquitecturas especificadas antes. Esto es necesario ya que JSONStore depende de una biblioteca tercera que sólo soporte estas arquitecturas.
   {: note}


### Abra la recopilación de JSONStore: Android
{: #open_android} 

Utilice `openCollections` para abrir una o varias recopilaciones de JSONStore.

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

### Obtener un descriptor de acceso para la recopilación de JSONStore
{: #get_jsonstore_android} 

Utilice `getCollectionByName` para crear un accesor a la recopilación. Es necesario llamar a `openCollections` antes de llamar a `getCollectionByName`.

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
La variable collection se puede utilizar ahora para realizar operaciones en la recopilación `people` como, por ejemplo, `add`, `find` y `replace`.

### Añadir documentos a una colección
{: #add_jsonstore_android} 

Utilice `addData` para almacenar datos como documentos dentro de una recopilación.

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

### Buscar documentos en una colección
{: #find_jsonstore_android} 

Utilice `findDocuments` para encontrar documentos dentro de una recopilación utilizando una consulta. Utilice `findAllDocuments` para recuperar todos los documentos dentro de una recopilación. Utilice `findDocumentById` para buscar documentos según su identificador exclusivo de documento.

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

### Sustituir documentos en una colección
{: #replace_jsonstore_android} 

Utilice `replaceDocuments` para modificar documentos dentro de una recopilación. El campo que se utiliza para realizar la sustitución es `_id`, el identificador exclusivo de documento.

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
En este ejemplo se supone que el documento `{_id: 1, json: {name: 'yoel', age: 23} }` está en la recopilación.

### Eliminar documentos de una recopilación
{: #remove_jsonstore_android} 

Utilice `removeDocumentById` para suprimir un documento de una recopilación. Los documentos no se quitan de la recopilación hasta que no llame a `markDocumentClean`. 

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

### Eliminar una recopilación completa
{: #remove_collection_jsonstore_android} 

Utilice `removeCollection` para suprimir todos los documentos que se almacenan dentro de una recopilación. Esta operación es similar a descartar una tabla en términos de una base de datos.

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

### Destruir JSONStore
{: #destroy_jsonstore_android} 

Utilice `destroy` para eliminar los siguientes datos:
* Todos los documentos
* Todas las recopilaciones
* Todos los almacenes
* Todos los artefactos de seguridad y metadatos de JSONStore

```java
Context context = getContext();
try {
  WLJSONStore.getInstance(context).destroy();
  // handle success
} catch(JSONStoreException e) {
  // handle failure
}
```

## Configuración avanzada de almacenamiento fuera de línea
{: #advanced_offline_storage}

Para obtener una configuración de almacenamiento fuera de línea, consulte [esto](advanced_jsonstore.html).
