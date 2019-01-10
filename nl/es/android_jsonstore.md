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

#	JSONStore en aplicaciones Android
{: #android_jsonstore}

## Requisitos previos
{: #prerequisites }

* Consulte la [visión general de JSONStore](jsonstore.html).
* Asegúrese de que el SDK nativo de MobileFirst se haya añadido al proyecto de Android Studio. Siga la guía de aprendizaje [Adición del SDK de MobileFirst a aplicaciones Android ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/android/).

## Adición de JSONStore
{: #adding-jsonstore }
1. En **Android → Scripts de Gradle**, seleccione el archivo **build.gradle (Módulo: app)**.

2. Añada lo siguiente a la sección de `dependencies` existente:
```
compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundationjsonstore:8.0.+'
```
{: codeblock}
3. Añada lo siguiente a la sección **DefaultConfig** del archivo `build.gradle`.
```
  ndk {
        abiFilters "armeabi", "armeabi-v7a", "x86", "mips"
      }
 ```     
 {: codeblock}
 > **Nota**: Añadimos *abiFilters* para garantizar que las aplicaciones que tengan JSONStore ejecuten en cualquiera de las arquitecturas especificadas. La adición de *abiFilters* es necesaria ya que JSONStore depende de una biblioteca de terceros que sólo da soporte a estas arquitecturas.

## Uso básico
{: #basic-usage }
### Abrir
{: #open }
Utilice `openCollections` para abrir una o varias recopilaciones de JSONStore.

Iniciar o aprovisionar una recopilación significa crear el almacenamiento persistente que contiene la recopilación y los documentos, si este no existe. Este almacenamiento persistente está cifrado y si se pasa una contraseña correcta, se ejecutan los procedimientos de seguridad necesarios para hacer que los datos estén accesibles.

Para obtener más información sobre las características opcionales que es posible habilitar en el tiempo de inicialización, consulte **Seguridad, Soporte a múltiples usuarios** e **Integración de adaptadores de MobileFirst** en la segunda parte de esta guía de aprendizaje.

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

### Obtener
{: #get }
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
{: codeblock}

La variable `collection` se puede utilizar ahora para realizar operaciones en la recopilación `people` como, por ejemplo, `add`, `find` y `replace`.

### Añadir
{: #add }
Utilice `addData` para almacenar datos como documentos dentro de la recopilación.

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

### Encontrar
{: #find }
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
{: codeblock}

### Sustituir
{: #replace }
Utilice `replaceDocument` para modificar documentos dentro de una recopilación. El campo que se utiliza para realizar la sustitución es `_id,` el identificador exclusivo de documento.

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

En este ejemplo se supone que el documento `{_id: 1, json: {name: 'yoel', age: 23} }` está en la recopilación.

### Eliminar
{: #remove }
Utilice `removeDocumentById` para suprimir un documento de una recopilación.
Los documentos no se quitan de la recopilación hasta que no llame a `markDocumentClean`. Para obtener más información, consulte la sección **Integración de adaptadores de MobileFirst** más adelante en esta sección.

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

### Eliminar recopilación
{: #remove-collection }
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
{: codeblock}

### Destruir
{: #destroy }
Utilice `destroy` para eliminar los siguientes datos:

* Todos los documentos
* Todas las recopilaciones
* Todos los almacenes - Consulte **Soporte a múltiples usuarios** más adelante en esta guía de aprendizaje
* Todos los artefactos de seguridad y metadatos de JSONStore - Consulte **Seguridad** más adelante en esta guía de aprendizaje

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

## Uso avanzado
{: #advanced-usage }
### Seguridad
{: #security }
Proteja todas las recopilaciones en un almacén pasando un objeto `JSONStoreInitOptions` con una contraseña para la función `openCollections`. Si no se pasa una contraseña, los documentos de todas las recopilaciones en el almacén no se cifran.

Algunos metadatos de seguridad se almacenan en las preferencias compartidas (Android).  
El almacén se cifra con una clave AES (Advanced Encryption Standard) de 256 bits. Todas las claves están reforzadas mediante PBKDF2 (Password-Based Key Derivation Function 2).

Utilice `closeAll` para bloquear el acceso a las recopilaciones hasta que llame a `openCollections` de nuevo. Si interpreta `openCollections` como una función de inicio de sesión, puede interpretar `closeAll` como la correspondiente función de finalización de sesión.

Utilice `changePassword` para cambiar la contraseña.

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

#### Soporte a múltiples usuarios
{: #multiple-user-support }
Es posible crear muchos almacenes con varias recopilaciones en una única aplicación de MobileFirst. La función `openCollections` puede tomar un objeto de opciones con un nombre de usuario. Si no se proporciona un nombre de usuario, el predeterminado es "**jsonstore**".

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

#### Integración del adaptador de MobileFirst
{: #mobilefirst-adapter-integration }
En esta sección se presupone que está familiarizado con los adaptadores. La integración del adaptador es opcional y proporciona formas de enviar datos desde una recopilación a un adaptador y obtener datos de dicho adaptador para la recopilación.
Si necesita más flexibilidad, también puede obtenerla utilizando funciones como, por ejemplo, `WLResourceRequest` o su propia instancia de `HttpClient`.

#### Implementación de adaptador
{: #adapter-implementation }
Cree un adaptador con el nombre "**JSONStoreAdapter**". Defina sus procedimientos `addPerson`, `getPeople`, `pushPeople`, `removePerson` y `replacePerson`.

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

#### Cargar datos desde el adaptador de MobileFirst
{: #load-data-from-mobilefirst-adapter }
Para cargar datos desde un adaptador utilice `WLResourceRequest`.

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

#### Obtener push necesario (documentos sucios)
{: #get-push-required-dirty-documents }
Al llamar a `findAllDirtyDocuments` se devuelve una matriz de los denominados "documentos sucios", que son documentos con modificaciones locales que no existen en el sistema de fondo.

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

Para evitar que JSONStore marque los documentos como "sucios", pase la opción `options.setMarkDirty(false)` para `add`, `replace` y `remove`.

#### Hacer push a los cambios
{: #push-changes }
Para hacer push a los cambios para un adaptador, llame a `findAllDirtyDocuments` para obtener una lista de documentos con modificaciones y, a continuación, utilizar `WLResourceRequest`. Después de enviar los datos y recibir una respuesta satisfactoria asegúrese de llamar a `markDocumentsClean`.

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

## Aplicación de ejemplo
{: #sample-application }
El proyecto `JSONStoreAndroid` contiene una aplicación Android nativa que utiliza la API de JSONStore.
Se incluye un proyecto de maven de adaptador JavaScript.

![Imagen de la aplicación de ejemplo](images/android-native-screen.jpg)

[Pulse para descargar](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreAndroid) el proyecto Android nativo.  
[Pulse para descargar](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreAdapter/tree/release80) el proyecto Maven del adaptador.  

### Uso de ejemplo
{: #sample-usage }
Siga el archivo README.md del ejemplo para obtener instrucciones.
