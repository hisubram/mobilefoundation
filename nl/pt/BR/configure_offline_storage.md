---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-12"

keywords: JSONStore, offline storage, add jsonstore to cordova, add jsonstore to iOS, add jsonstore to android, jsonstore methods, jsonstore operations

subcollection:  mobilefoundation
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

# Configurar o armazenamento off-line
{: #configure_offline_storage}

O Mobile Foundation JSONStore é uma API opcional do lado do cliente que fornece um sistema de armazenamento leve e orientado a documentos. O JSONStore ativa o armazenamento persistente de documentos JSON. Os documentos em um aplicativo ficam disponíveis no JSONStore mesmo quando o dispositivo que está executando o aplicativo está off-line. Esse armazenamento persistente e sempre disponível pode ser útil para fornecer aos usuários acesso aos documentos quando, por exemplo, não há nenhuma conexão de rede disponível no dispositivo. Para obter uma visão geral dos conceitos e da terminologia do JSONStore, veja [aqui](/docs/services/mobilefoundation?topic=mobilefoundation-jsonstore#jsonstore).

Para configuração avançada de armazenamento off-line, veja [aqui](/docs/services/mobilefoundation?topic=mobilefoundation-advanced_jsonstore#advanced_jsonstore).
{: note}

### Configurar o armazenamento off-line para os apps Cordova ou Ionic
{: #configure_offline_storage_cordova}
{: cordova}

Certifique-se de que o Mobile Foundation Cordova SDK tenha sido incluído no projeto.
{: cordova}

Siga o tutorial [Incluindo o Mobile Foundation SDK em aplicativos Cordova ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/cordova/).
{: tip}
{: cordova}

#### Incluindo o JSONStore em seu projeto Cordova
{: #adding_jsonstore_cordova}
{: cordova}

1. Abra uma janela de linha de comandos e navegue para a pasta do projeto Cordova.
2. Execute o comando:
   ```bash
   cordova plugin add cordova-plugin-mfp-jsonstore
   ```
   {: codeblock}
   {: cordova}

#### Inicializar a coleção do JSONStore
{: #initialize_jsonstore_cordova}   
{: cordova}

Use `init` para iniciar uma ou mais coleções do JSONStore.
Iniciar ou provisionar uma coleção significa criar o armazenamento persistente que contém a coleção e os documentos, se ele não existir. Se uma senha for passada para opções, o armazenamento persistente será criptografado com a senha.
{: cordova}

```javascript
var collections = {
    people : {
        searchFields: {name: 'string', age: 'integer'}
    }
}; WL.JSONStore.init (coleções) .then (function (collections) {; WL.JSONStore.init (coleções) .then (function (collections) {
    // handle success - collection.people (people's collection)
}).fail(function (error) {
    // handle failure });
```
{: codeblock}
{: cordova}

#### Obtenha um acessador para a sua coleção do Cordova JSONStore
{: #get_jsonstore_cordova}
{: cordova}

Use `get` para criar um acessador para a coleção. Deve-se chamar `init` antes de chamar get, caso contrário, o resultado de `get` será *indefinido*.
{: cordova}
```javascript
var collectionName = 'people'; var people = WL.JSONStore.get(collectionName);
```
{: codeblock}
{: cordova}

A variável *people* pode agora ser usada para executar operações na coleção *people*, como `add`, `find` e `replace`.
{: cordova}

#### Inclua documentos em uma coleção do Cordova
{: #add_jsonstore_cordova}
{: cordova}

Use `add` para armazenar dados como documentos dentro de uma coleção.
{: cordova}

```javascript
var collectionName = 'people'; var options = {}; var data = {name: 'yoel', age: 23};

WL.JSONStore.get(collectionName).add(data, options).then(function () {
    // handle success }).fail(function (error) {
    // handle failure });
```
{: codeblock}
{: cordova}

#### Localize documentos dentro de uma coleção do Cordova
{: #find_jsonstore_cordova}
{: cordova}

* Use `find` para localizar um documento dentro de uma coleção usando uma consulta.
* Use `findAll` para recuperar todos os documentos dentro de uma coleção.
* Use `findById` para procurar pelo identificador exclusivo de documento.
{: cordova}

O comportamento padrão para localizar é fazer uma procura "difusa".
{: cordova}

```javascript
var query = {name: 'yoel'}; var collectionName = 'people'; var options = {
  exact: false, //default limit: 10 // returns a maximum of 10 documents, default: return every document };

WL.JSONStore.get(collectionName).find(query, options).then(function (results) {
    // handle success - results (array of documents found)
}).fail(function (error) {
    // handle failure });
```
{: codeblock}
{: cordova}

```javascript
var age = document.getElementById("findByAge").value || '';

if(age == "" || isNaN(age)){ alert("Please enter a valid age to find"); } else {
  query = {age: parseInt(age, 10)}; var options = {
    exact: true, limit: 10 //returns a maximum of 10 documents }; WL.JSONStore.get(collectionName).find(query, options).then(function (res) {
    // handle success - results (array of documents found)
  }).fail(function (errorObject) {
    // handle failure }); }
```
{: codeblock}
{: cordova}

#### Substitua documentos dentro de uma coleção do Cordova
{: #replace_jsonstore_cordova}
{: cordova}

Use `replace` para modificar documentos dentro de uma coleção. O campo usado para executar a substituição é `_id`, o identificador exclusivo do documento.
{: cordova}

```javascript
var document = {
  _id: 1, json: {name: 'chevy', age: 23}
}; var collectionName = 'people'; var options = {};

WL.JSONStore.get(collectionName).replace(document, options).then(function (numberOfDocsReplaced) {
    // handle success }).fail(function (error) {
    // handle failure });
```
{: codeblock}
{: cordova}

Esse exemplo supõe que o documento `{_id: 1, json: {name: 'yoel', age: 23} }` está na coleção.
{: cordova}

#### Remova documentos de uma coleção do Cordova
{: #remove_jsonstore_cordova}
{: cordova}

Use `remove` para excluir um documento de uma coleção.
Os documentos não são apagados da coleção até que você chame push.
{: cordova}

```javascript
var query = {_id: 1}; var collectionName = 'people'; var options = {exact: true}; WL.JSONStore.get(collectionName).remove(query, options).then(function (numberOfDocsRemoved) {
    // handle success }).fail(function (error) {
    // handle failure });
```
{: codeblock}
{: cordova}

#### Remova uma coleção inteira do Cordova
{: #remove_collection_jsonstore_cordova}
{: cordova}

Use `removeCollection` para excluir todos os documentos armazenados dentro de uma coleção. Essa operação é semelhante ao descarte de uma tabela em termos de banco de dados.
{: cordova}

#### Destrua o Cordova JSONStore
{: #destroy_jsonstore_cordova}
{: cordova}

Use `destroy` para remover os dados a seguir:
* Todos os documentos
* Todas as coleções
* Todos os armazenamentos
* Todos os metadados e artefatos de segurança JSONStore
{: cordova}

### Configurar o armazenamento off-line para apps iOS
{: #configure_offline_storage_ios}
{: ios}

Certifique-se de que o Mobile Foundation Native SDK tenha sido incluído no projeto Xcode.
{: ios}

Siga o tutorial [Incluindo o Mobile Foundation SDK em aplicativos iOS ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/ios/).
{: tip}
{: ios}

#### Incluindo o JSONStore em seu projeto iOS
{: #adding_jsonstore_ios}
{: ios}

1. Inclua o seguinte no `podfile` existente, na raiz do projeto Xcode.
   ```bash
   pod 'IBMMobileFirstPlatformFoundationJSONStore'
   ```
   {: codeblock}
   {: ios}
2. Na linha de comandos, acesse a raiz do projeto Xcode e execute o comando:
   ```bash
   instalação do pod
   ```
   {: codeblock}
   {: ios}
3. Sempre que você desejar usar o JSONStore, certifique-se de importar o cabeçalho do JSONStore:
**Objective-C**:
   ```objectivec
   #import <IBMMobileFirstPlatformFoundationJSONStore/IBMMobileFirstPlatformFoundationJSONStore.h>
   ```
   {: codeblock}
   **Swift:**
   ```swift
   import IBMMobileFirstPlatformFoundationJSONStore
   ```   
   {: codeblock}
   {: ios}

#### Abra a coleção do iOS JSONStore
{: #open_ios}
{: ios}

Use `openCollections` para abrir uma ou mais coleções JSONStore.
{: ios}

```swift
let collection:JSONStoreCollection = JSONStoreCollection(name: "people") collection.setSearchField("name", withType: JSONStore_String) collection.setSearchField("age", withType: JSONStore_Integer)

fazer {
  try JSONStore.sharedInstance().openCollections([collection], withOptions: nil)
} catch let error as NSError {
  // handle error }
```
{: codeblock}
{: ios}

#### Obtenha um acessador para a sua coleção do iOS JSONStore
{: #get_jsonstore_ios}
{: ios}

Use `getCollectionWithName` para criar um acessador para a coleção. Deve-se chamar `openCollections` antes de chamar `getCollectionWithName`.
{: ios}

```swift
let collectionName:String = "people" let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)
```
{: codeblock}
{: ios}

A coleção de variáveis agora pode ser usada para executar operações na coleção `people`, como `add`, `find` e `replace`.
{: ios}

#### Inclua documentos em uma coleção do iOS
{: #add_jsonstore_ios}
{: ios}

Use `addData` para armazenar dados como documentos dentro de uma coleção.
{: ios}

```swift
let collectionName:String = "people" let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

let data = ["name" : "yoel", "age" : 23]

fazer {
  try collection.addData([data], andMarkDirty: true, withOptions: nil)
} catch let error as NSError {
  // handle error }
```
{: codeblock}
{: ios}

#### Localize documentos dentro de uma coleção do iOS
{: #find_jsonstore_ios}
{: ios}

Use `findWithQueryParts` para localizar um documento dentro de uma coleção usando uma consulta. Use `findAllWithOptions` para recuperar todos os documentos dentro de uma coleção. Use `findWithIds` para procurar pelo identificador exclusivo do documento.
{: ios}

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
{: codeblock}
{: ios}

#### Substitua documentos dentro de uma coleção do iOS
{: #replace_jsonstore_ios}
{: ios}

Use `replaceDocuments` para modificar documentos dentro de uma coleção. O campo usado para executar a substituição é `_id`, o identificador exclusivo do documento.
{: ios}

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
{: codeblock}
{: ios}

Esse exemplo supõe que o documento `{_id: 1, json: {name: 'yoel', age: 23} }` está na coleção.
{: ios}

#### Remova documentos de uma coleção do iOS
{: #remove_jsonstore_ios}
{: ios}

Use `removeWithIds` para excluir um documento de uma coleção. Os documentos não são apagados da coleção até que você chame `markDocumentClean`.
{: ios}

```swift
let collectionName:String = "people" let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

fazer {
  try collection.removeWithIds([1], andMarkDirty: true)
} catch let error as NSError {
  // handle error }
```
{: codeblock}
{: ios}

#### Remova uma coleção inteira do iOS
{: #remove_collection_jsonstore_ios}
{: ios}

Use `removeCollection` para excluir todos os documentos armazenados dentro de uma coleção. Essa operação é semelhante ao descarte de uma tabela em termos de banco de dados.
{: ios}

```swift
let collectionName:String = "people" let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

fazer {
  try collection.removeCollection()
} catch let error as NSError {
  // handle error }
```
{: codeblock}
{: ios}

#### Destrua um iOS JSONStore
{: #destroy_jsonstore_ios}
{: ios}

Use `destroyData` para remover os dados a seguir:
* Todos os documentos
* Todas as coleções
* Todos os armazenamentos
* Todos os metadados e artefatos de segurança JSONStore
{: ios}

```swift
fazer {
  try JSONStore.sharedInstance().destroyData()
} catch let error as NSError {
  // handle error }
```
{: codeblock}
{: ios}

### Configurar o armazenamento off-line para apps Android
{: #configure_offline_storage_android}
{: android}

Certifique-se de que o Mobile Foundation Native SDK tenha sido incluído no projeto Android Studio.
{: android}

Siga o tutorial [Incluindo o Mobile Foundation SDK em aplicativos Android ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/android/).
{: tip}
{: android}

#### Incluindo o JSONStore em seu projeto Android
{: #adding_jsonstore_android}
{: android}

1. Em **Android → Scripts do Gradle**, selecione o arquivo `build.gradle (Módulo: app)`.
2. Incluir o seguinte na seção `dependencies` existente:
   ```bash
   compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundationjsonstore:8.0.+'
   ```
   {: codeblock}
   {: android}
3. Inclua o seguinte na seção `DefaultConfig` de seu arquivo `build.gradle`:
   ```gradle
   ndk {
     abiFilters "armeabi", "armeabi-v7a", "x86", "mips" }
   ```
   {: codeblock}
   {: android}
   Nós incluímos o `abiFilters` para assegurar que os apps que têm o JSONStore sejam executados em qualquer uma das arquiteturas especificadas acima. Isso é necessário, pois o JSONStore é dependente de uma biblioteca de terceiro que suporta somente essas arquiteturas.
   {: note}
   {: android}

#### Abra uma coleção do Android JSONStore
{: #open_android}
{: android}

Use `openCollections` para abrir uma ou mais coleções JSONStore.
{: android}

```java
Context context = getContext(); try {
  JSONStoreCollection people = new JSONStoreCollection("people"); people.setSearchField("name", SearchFieldType.STRING); people.setSearchField("age", SearchFieldType.INTEGER); List<JSONStoreCollection> collections = new LinkedList<JSONStoreCollection>(); collections.add(people); WLJSONStore.getInstance(context).openCollections(collections); // sucesso de manipulação } catch(JSONStoreException e) {
  // handle failure }
```
{: codeblock}
{: android}

#### Obtenha um acessador para a sua coleção do Android JSONStore
{: #get_jsonstore_android}
{: android}

Use `getCollectionByName` para criar um acessador para a coleção. Deve-se chamar `openCollections` antes de chamar `getCollectionByName`.
{: android}

```java
Context context = getContext(); try {
  String collectionName = "people"; JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName); // handle success } catch(JSONStoreException e) {
  // handle failure }
```
{: codeblock}
{: android}

A coleção de variáveis agora pode ser usada para executar operações na coleção `people`, como `add`, `find` e `replace`.
{: android}

#### Inclua documentos em uma coleção do Android
{: #add_jsonstore_android}
{: android}

Use `addData` para armazenar dados como documentos dentro de uma coleção.
{: android}

```java
Context context = getContext(); try {
  String collectionName = "people"; JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName); //Add options.
  JSONStoreAddOptions options = new JSONStoreAddOptions(); options.setMarkDirty(true); JSONObject data = new JSONObject("{age: 23, name: 'yoel'}") collection.addData(data, options); // handle success } catch(JSONStoreException e) {
  // handle failure }
```
{: codeblock}
{: android}

#### Localize documentos dentro de uma coleção do Android
{: #find_jsonstore_android}
{: android}

Use `findDocuments` para localizar um documento dentro de uma coleção usando uma consulta. Use `findAllDocuments` para recuperar todos os documentos dentro de uma coleção. Use `findDocumentById` para procurar pelo identificador exclusivo do documento.
{: android}

```java
Context context = getContext(); try {
  String collectionName = "people"; JSONStoreQueryPart queryPart = new JSONStoreQueryPart(); // fuzzy search LIKE queryPart.addLike("name", name); JSONStoreQueryParts query = new JSONStoreQueryParts(); query.addQueryPart(queryPart); JSONStoreFindOptions options = new JSONStoreFindOptions(); // returns a maximum of 10 documents, default: returns every document options.setLimit(10); JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName); List<JSONObject> results = collection.findDocuments(query, options); // handle success } catch(JSONStoreException e) {
  // handle failure }
```
{: codeblock}
{: android}

#### Substitua documentos dentro de uma coleção do Android
{: #replace_jsonstore_android}
{: android}

Use `replaceDocuments` para modificar documentos dentro de uma coleção. O campo usado para executar a substituição é `_id`, o identificador exclusivo do documento.
{: android}

```java
Context context = getContext(); try {
  String collectionName = "people"; JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName); JSONStoreReplaceOptions options = new JSONStoreReplaceOptions(); // mark data as dirty options.setMarkDirty(true); JSONStore replacement = new JSONObject("{_id: 1, json: {age: 23, name: 'chevy'}}"); collection.replaceDocument(replacement, options); // handle success } catch(JSONStoreException e) {
  // handle failure }
```
{: codeblock}
{: android}

Esse exemplo supõe que o documento `{_id: 1, json: {name: 'yoel', age: 23} }` está na coleção.
{: android}

#### Remova documentos de uma coleção do Android
{: #remove_jsonstore_android}
{: android}

Use `removeDocumentById` para excluir um documento de uma coleção. Os documentos não são apagados da coleção até que você chame `markDocumentClean`.
{: android}

```java
Context context = getContext(); try {
  String collectionName = "people"; JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName); JSONStoreRemoveOptions options = new JSONStoreRemoveOptions(); // Mark data as dirty options.setMarkDirty(true); collection.removeDocumentById(1, options); // handle success } catch(JSONStoreException e) {
  // handle failure }
```
{: codeblock}
{: android}

#### Remova uma coleção inteira do Android
{: #remove_collection_jsonstore_android}
{: android}

Use `removeCollection` para excluir todos os documentos armazenados dentro de uma coleção. Essa operação é semelhante ao descarte de uma tabela em termos de banco de dados.
{: android}

```java
Context context = getContext(); try {
  String collectionName = "people"; JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName); collection.removeCollection(); // handle success } catch(JSONStoreException e) {
  // handle failure }
```
{: codeblock}
{: android}

#### Destrua um Android JSONStore
{: #destroy_jsonstore_android}
{: android}

Use `destroy` para remover os dados a seguir:
* Todos os documentos
* Todas as coleções
* Todos os armazenamentos
* Todos os metadados e artefatos de segurança JSONStore
{: android}

```java
Context context = getContext(); try {
  WLJSONStore.getInstance(context).destroy(); // handle success } catch(JSONStoreException e) {
  // handle failure }
```
{: codeblock}
{: android}
