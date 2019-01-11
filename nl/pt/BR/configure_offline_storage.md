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

# Configurar o armazenamento off-line
{: #configure_offline_storage}

O Mobile Foundation JSONStore é uma API opcional do lado do cliente que fornece um sistema de armazenamento leve e orientado a documentos. O JSONStore ativa o armazenamento persistente de documentos JSON. Os documentos em um aplicativo ficam disponíveis no JSONStore mesmo quando o dispositivo que está executando o aplicativo está off-line. Esse armazenamento persistente e sempre disponível pode ser útil para fornecer aos usuários acesso aos documentos quando, por exemplo, não há nenhuma conexão de rede disponível no dispositivo. Para obter uma visão geral dos conceitos e da terminologia do JSONStore, veja [aqui](jsonstore.html).

## Configurar o armazenamento off-line para os apps Cordova ou Ionic
{: #configure_offline_storage_cordova}

Certifique-se de que o Mobile Foundation Cordova SDK tenha sido incluído no projeto. 

Siga o tutorial [Incluindo o Mobile Foundation SDK em aplicativos Cordova ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/cordova/).
{: tip}

### Incluindo o JSONStore em seu projeto Cordova
{: #adding_jsonstore_cordova}

1. Abra uma janela de linha de comandos e navegue para a pasta do projeto Cordova.
2. Execute o comando: 
   ```bash
   cordova plugin add cordova-plugin-mfp-jsonstore
   ```

### Inicializar a coleção do JSONStore
{: #initialize_jsonstore_cordova}   

Use `init` para iniciar uma ou mais coleções do JSONStore.
Iniciar ou provisionar uma coleção significa criar o armazenamento persistente que contém a coleção e os documentos, se ele não existir. Se uma senha for passada para opções, o armazenamento persistente será criptografado com a senha.

```javascript
var collections = {
    people : {
        searchFields: {name: 'string', age: 'integer'}
    }
};

WL.JSONStore.init(collections).then(function (collections) {
    // handle success - collection.people (people's collection)
}).fail(function (error) {
    // handle failure });
```

### Obter um acessador para sua coleção JSONStore
{: #get_jsonstore_cordova} 

Use `get` para criar um acessador para a coleção. Deve-se chamar `init` antes de chamar get, caso contrário, o resultado de `get` será *indefinido*.

```javascript
var collectionName = 'people'; var people = WL.JSONStore.get(collectionName);
```
A variável *people* pode agora ser usada para executar operações na coleção *people*, como `add`, `find` e `replace`.

### Incluir documentos em uma coleção
{: #add_jsonstore_cordova} 

Use `add` para armazenar dados como documentos dentro de uma coleção.

```javascript
var collectionName = 'people'; var options = {}; var data = {name: 'yoel', age: 23};

WL.JSONStore.get(collectionName).add(data, options).then(function () {
    // handle success }).fail(function (error) {
    // handle failure });
```

### Localizar documentos dentro de uma coleção
{: #find_jsonstore_cordova} 

* Use `find` para localizar um documento dentro de uma coleção usando uma consulta.
* Use `findAll` para recuperar todos os documentos dentro de uma coleção.
* Use `findById` para procurar pelo identificador exclusivo de documento.

O comportamento padrão para localizar é fazer uma procura "difusa".

```javascript
var query = {name: 'yoel'}; var collectionName = 'people'; var options = {
  exact: false, //default limit: 10 // returns a maximum of 10 documents, default: return every document };

WL.JSONStore.get(collectionName).find(query, options).then(function (results) {
    // handle success - results (array of documents found)
}).fail(function (error) {
    // handle failure });
```

```javascript
var age = document.getElementById("findByAge").value || '';

if(age == "" || isNaN(age)){ alert("Please enter a valid age to find"); } else {
  query = {age: parseInt(age, 10)}; var options = {
    exact: true, limit: 10 //returns a maximum of 10 documents }; WL.JSONStore.get(collectionName).find(query, options).then(function (res) {
    // handle success - results (array of documents found)
  }).fail(function (errorObject) {
    // handle failure }); }
```

### Substituir documentos dentro de uma coleção
{: #replace_jsonstore_cordova} 

Use `replace` para modificar documentos dentro de uma coleção. O campo usado para executar a substituição é `_id`, o identificador exclusivo do documento.

```javascript
var document = {
  _id: 1, json: {name: 'chevy', age: 23}
}; var collectionName = 'people'; var options = {};

WL.JSONStore.get(collectionName).replace(document, options).then(function (numberOfDocsReplaced) {
    // handle success }).fail(function (error) {
    // handle failure });
```
Esse exemplo supõe que o documento `{_id: 1, json: {name: 'yoel', age: 23} }` está na coleção.


### Remover documentos de uma coleção
{: #remove_jsonstore_cordova} 

Use `remove` para excluir um documento de uma coleção.
Os documentos não são apagados da coleção até que você chame push.

```javascript
var query = {_id: 1}; var collectionName = 'people'; var options = {exact: true}; WL.JSONStore.get(collectionName).remove(query, options).then(function (numberOfDocsRemoved) {
    // handle success }).fail(function (error) {
    // handle failure });
```

### Remover uma coleção inteira
{: #remove_collection_jsonstore_cordova} 

Use `removeCollection` para excluir todos os documentos armazenados dentro de uma coleção. Essa operação é semelhante ao descarte de uma tabela em termos de banco de dados.

### Destruir JSONStore
{: #destroy_jsonstore_cordova} 

Use `destroy` para remover os dados a seguir:
* Todos os documentos
* Todas as coleções
* Todos os armazenamentos
* Todos os metadados e artefatos de segurança JSONStore

## Configurar o armazenamento off-line para apps iOS
{: #configure_offline_storage_ios}

Certifique-se de que o Mobile Foundation Native SDK tenha sido incluído no projeto Xcode. 

Siga o tutorial [Incluindo o Mobile Foundation SDK em aplicativos iOS ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/ios/).
{: tip}

### Incluindo o JSONStore em seu projeto iOS
{: #adding_jsonstore_ios}

1. Inclua o seguinte no `podfile` existente, na raiz do projeto Xcode.
   ```bash
   pod 'IBMMobileFirstPlatformFoundationJSONStore'
   ```
2. Na linha de comandos, acesse a raiz do projeto Xcode e execute o comando: 
   ```bash
   instalação do pod
   ``` 
3. Sempre que você desejar usar o JSONStore, certifique-se de importar o cabeçalho do JSONStore:
**Objective-C**:
   ```objectivec
   #import <IBMMobileFirstPlatformFoundationJSONStore/IBMMobileFirstPlatformFoundationJSONStore.h>
   ``` 
   **Swift:**
   ```swift
   import IBMMobileFirstPlatformFoundationJSONStore
   ```   

### Abrir a coleção do JSONStore: iOS
{: #open_ios} 

Use `openCollections` para abrir uma ou mais coleções JSONStore.

```swift
let collection:JSONStoreCollection = JSONStoreCollection(name: "people") collection.setSearchField("name", withType: JSONStore_String) collection.setSearchField("age", withType: JSONStore_Integer)

fazer {
  try JSONStore.sharedInstance().openCollections([collection], withOptions: nil)
} catch let error as NSError {
  // handle error }
```

### Obter um acessador para sua coleção JSONStore
{: #get_jsonstore_ios} 

Use `getCollectionWithName` para criar um acessador para a coleção. Deve-se chamar `openCollections` antes de chamar `getCollectionWithName`.

```swift
let collectionName:String = "people" let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)
```
A coleção de variáveis agora pode ser usada para executar operações na coleção `people`, como `add`, `find` e `replace`.

### Incluir documentos em uma coleção
{: #add_jsonstore_ios} 

Use `addData` para armazenar dados como documentos dentro de uma coleção.

```swift
let collectionName:String = "people" let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

let data = ["name" : "yoel", "age" : 23]

fazer {
  try collection.addData([data], andMarkDirty: true, withOptions: nil)
} catch let error as NSError {
  // handle error }
```

### Localizar documentos dentro de uma coleção
{: #find_jsonstore_ios} 

Use `findWithQueryParts` para localizar um documento dentro de uma coleção usando uma consulta. Use `findAllWithOptions` para recuperar todos os documentos dentro de uma coleção. Use `findWithIds` para procurar pelo identificador exclusivo do documento.

```swift
let collectionName:String = "people" let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

let options:JSONStoreQueryOptions = JSONStoreQueryOptions()
// returns a maximum of 10 documents, default: returns every document
options.limit = 10

let query:JSONStoreQueryPart = JSONStoreQueryPart()
query.searchField("name", like: "yoel")

fazer {
  let results:NSArray = try collection.findWithQueryParts([query], andOptions: options)
} catch let error as NSError {
  // handle error }
```

### Substituir documentos dentro de uma coleção
{: #replace_jsonstore_ios} 

Use `replaceDocuments` para modificar documentos dentro de uma coleção. O campo usado para executar a substituição é `_id`, o identificador exclusivo do documento.

```swift
let collectionName:String = "people" let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

var document:Dictionary<String,AnyObject> = Dictionary()
document["name"] = "chevy"
document["age"] = 23

var replacement:Dictionary<String, AnyObject> = Dictionary()
replacement["_id"] = 1
replacement["json"] = document

fazer {
  try collection.replaceDocuments([replacement], andMarkDirty: true)
} catch let error as NSError {
  // handle error }
```
Esse exemplo supõe que o documento `{_id: 1, json: {name: 'yoel', age: 23} }` está na coleção.

### Remover documentos de uma coleção
{: #remove_jsonstore_ios} 

Use `removeWithIds` para excluir um documento de uma coleção. Os documentos não são apagados da coleção até que você chame `markDocumentClean`. 

```swift
let collectionName:String = "people" let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

fazer {
  try collection.removeWithIds([1], andMarkDirty: true)
} catch let error as NSError {
  // handle error }
```

### Remover uma coleção inteira
{: #remove_collection_jsonstore_ios} 

Use `removeCollection` para excluir todos os documentos armazenados dentro de uma coleção. Essa operação é semelhante ao descarte de uma tabela em termos de banco de dados.

```swift
let collectionName:String = "people" let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

fazer {
  try collection.removeCollection()
} catch let error as NSError {
  // handle error }
```

### Destruir JSONStore
{: #destroy_jsonstore_ios} 

Use `destroyData` para remover os dados a seguir:
* Todos os documentos
* Todas as coleções
* Todos os armazenamentos
* Todos os metadados e artefatos de segurança JSONStore

```swift
fazer {
  try JSONStore.sharedInstance().destroyData()
} catch let error as NSError {
  // handle error }
```

## Configurar o armazenamento off-line para apps Android
{: #configure_offline_storage_android}

Certifique-se de que o Mobile Foundation Native SDK tenha sido incluído no projeto Android Studio. 

Siga o tutorial [Incluindo o Mobile Foundation SDK em aplicativos Android ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/android/).
{: tip}

### Incluindo o JSONStore em seu projeto Android
{: #adding_jsonstore_android}

1. Em **Android → Scripts do Gradle**, selecione o arquivo `build.gradle (Módulo: app)`.
2. Incluir o seguinte na seção `dependencies` existente: 
   ```bash
   compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundationjsonstore:8.0.+'
   ``` 
3. Inclua o seguinte na seção `DefaultConfig` de seu arquivo `build.gradle`:
   ```gradle
   ndk {
     abiFilters "armeabi", "armeabi-v7a", "x86", "mips" }
   ``` 
   Nós incluímos o `abiFilters` para assegurar que os apps que têm o JSONStore sejam executados em qualquer uma das arquiteturas especificadas acima. Isso é necessário, pois o JSONStore é dependente de uma biblioteca de terceiro que suporta somente essas arquiteturas.
   {: note}


### Abrir a coleção do JSONStore: Android
{: #open_android} 

Use `openCollections` para abrir uma ou mais coleções JSONStore.

```java
Context context = getContext(); try {
  JSONStoreCollection people = new JSONStoreCollection("people"); people.setSearchField("name", SearchFieldType.STRING); people.setSearchField("age", SearchFieldType.INTEGER); List<JSONStoreCollection> collections = new LinkedList<JSONStoreCollection>(); collections.add(people); WLJSONStore.getInstance(context).openCollections(collections); // sucesso de manipulação } catch(JSONStoreException e) {
  // handle failure }
```

### Obter um acessador para sua coleção JSONStore
{: #get_jsonstore_android} 

Use `getCollectionByName` para criar um acessador para a coleção. Deve-se chamar `openCollections` antes de chamar `getCollectionByName`.

```java
Context context = getContext(); try {
  String collectionName = "people"; JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName); // handle success } catch(JSONStoreException e) {
  // handle failure }
```
A coleção de variáveis agora pode ser usada para executar operações na coleção `people`, como `add`, `find` e `replace`.

### Incluir documentos em uma coleção
{: #add_jsonstore_android} 

Use `addData` para armazenar dados como documentos dentro de uma coleção.

```java
Context context = getContext(); try {
  String collectionName = "people"; JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName); //Add options.
  JSONStoreAddOptions options = new JSONStoreAddOptions(); options.setMarkDirty(true); JSONObject data = new JSONObject("{age: 23, name: 'yoel'}") collection.addData(data, options); // handle success } catch(JSONStoreException e) {
  // handle failure }
```

### Localizar documentos dentro de uma coleção
{: #find_jsonstore_android} 

Use `findDocuments` para localizar um documento dentro de uma coleção usando uma consulta. Use `findAllDocuments` para recuperar todos os documentos dentro de uma coleção. Use `findDocumentById` para procurar pelo identificador exclusivo do documento.

```java
Context context = getContext(); try {
  String collectionName = "people"; JSONStoreQueryPart queryPart = new JSONStoreQueryPart(); // fuzzy search LIKE queryPart.addLike("name", name); JSONStoreQueryParts query = new JSONStoreQueryParts(); query.addQueryPart(queryPart); JSONStoreFindOptions options = new JSONStoreFindOptions(); // returns a maximum of 10 documents, default: returns every document options.setLimit(10); JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName); List<JSONObject> results = collection.findDocuments(query, options); // handle success } catch(JSONStoreException e) {
  // handle failure }
```

### Substituir documentos dentro de uma coleção
{: #replace_jsonstore_android} 

Use `replaceDocuments` para modificar documentos dentro de uma coleção. O campo usado para executar a substituição é `_id`, o identificador exclusivo do documento.

```java
Context context = getContext(); try {
  String collectionName = "people"; JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName); JSONStoreReplaceOptions options = new JSONStoreReplaceOptions(); // mark data as dirty options.setMarkDirty(true); JSONStore replacement = new JSONObject("{_id: 1, json: {age: 23, name: 'chevy'}}"); collection.replaceDocument(replacement, options); // handle success } catch(JSONStoreException e) {
  // handle failure }
```
Esse exemplo supõe que o documento `{_id: 1, json: {name: 'yoel', age: 23} }` está na coleção.

### Remover documentos de uma coleção
{: #remove_jsonstore_android} 

Use `removeDocumentById` para excluir um documento de uma coleção. Os documentos não são apagados da coleção até que você chame `markDocumentClean`. 

```java
Context context = getContext(); try {
  String collectionName = "people"; JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName); JSONStoreRemoveOptions options = new JSONStoreRemoveOptions(); // Mark data as dirty options.setMarkDirty(true); collection.removeDocumentById(1, options); // handle success } catch(JSONStoreException e) {
  // handle failure }
```

### Remover uma coleção inteira
{: #remove_collection_jsonstore_android} 

Use `removeCollection` para excluir todos os documentos armazenados dentro de uma coleção. Essa operação é semelhante ao descarte de uma tabela em termos de banco de dados.

```java
Context context = getContext(); try {
  String collectionName = "people"; JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName); collection.removeCollection(); // handle success } catch(JSONStoreException e) {
  // handle failure }
```

### Destruir JSONStore
{: #destroy_jsonstore_android} 

Use `destroy` para remover os dados a seguir:
* Todos os documentos
* Todas as coleções
* Todos os armazenamentos
* Todos os metadados e artefatos de segurança JSONStore

```java
Context context = getContext(); try {
  WLJSONStore.getInstance(context).destroy(); // handle success } catch(JSONStoreException e) {
  // handle failure }
```

## Configuração avançada do armazenamento off-line
{: #advanced_offline_storage}

Para configuração avançada de armazenamento off-line, veja [aqui](advanced_jsonstore.html).
