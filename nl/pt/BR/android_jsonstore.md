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

#	JSONStore em aplicativos Android
{: #android_jsonstore}

## Pré-requisitos
{: #prerequisites }

* Ler a [visão geral de JSONStore](jsonstore.html).
* Certifique-se de que o MobileFirst Native SDK tenha sido incluído no projeto Android Studio. Siga o tutorial [Incluindo o MobileFirst SDK em aplicativos Android ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/android/).

## Incluindo JSONStore
{: #adding-jsonstore }
1. Em **Android → Scripts do Gradle**, selecione o arquivo **build.gradle (Módulo: app)**.

2. Incluir o seguinte na seção `dependencies` existente:
```
compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundationjsonstore:8.0.+'
```
{: codeblock}
3. Inclua o seguinte na seção **DefaultConfig** de seu arquivo `build.gradle`.
```
  ndk {
        abiFilters "armeabi", "armeabi-v7a", "x86", "mips" }
 ```     
 {: codeblock}
 > **Nota**: nós incluímos os *abiFilters* para assegurar que os apps que têm o JSONStore sejam executados em qualquer uma das arquiteturas especificadas. A inclusão de *abiFilters* é necessária porque o JSONStore depende de uma biblioteca de terceiros, que suporta somente essas arquiteturas.

## Uso básico
{: #basic-usage }
### Abrir
{: #open }
Use `openCollections` para abrir uma ou mais coleções JSONStore.

Iniciar ou provisionar uma coleção significa criar o armazenamento persistente que contém a coleção e os documentos, se ele não existir. Se o armazenamento persistente estiver criptografado e uma senha correta for passada, os procedimentos de segurança necessários para tornar os dados acessíveis serão executados.

Para recursos opcionais que podem ser ativados no momento da inicialização, veja **Segurança, suporte a múltiplos usuários** e **Integração do adaptador MobileFirst** na segunda parte deste tutorial.

```java
Context context = getContext(); try {
  JSONStoreCollection people = new JSONStoreCollection("people"); people.setSearchField("name", SearchFieldType.STRING); people.setSearchField("age", SearchFieldType.INTEGER); List<JSONStoreCollection> collections = new LinkedList<JSONStoreCollection>(); collections.add(people); WLJSONStore.getInstance(context).openCollections(collections); // sucesso de manipulação } catch(JSONStoreException e) {
  // handle failure }
```
{: codeblock}

### Obter
{: #get }
Use `getCollectionByName` para criar um acessador para a coleção. Deve-se chamar `openCollections` antes de chamar `getCollectionByName`.

```java
Context context = getContext(); try {
  String collectionName = "people"; JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName); // handle success } catch(JSONStoreException e) {
  // handle failure }
```
{: codeblock}

A variável `collection` pode agora ser usada para executar operações na coleção `people`, como `add`, `find` e `replace`

### Incluir
{: #add }
Use `addData` para armazenar dados como documentos dentro de uma coleção

```java
Context context = getContext(); try {
  String collectionName = "people"; JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName); //Add options.
  JSONStoreAddOptions options = new JSONStoreAddOptions(); options.setMarkDirty(true); JSONObject data = new JSONObject("{age: 23, name: 'yoel'}") collection.addData(data, options); // handle success } catch(JSONStoreException e) {
  // handle failure }
```
{: codeblock}

### Localizar
{: #find }
Use `findDocuments` para localizar um documento dentro de uma coleção usando uma consulta. Use `findAllDocuments` para recuperar todos os documentos dentro de uma coleção. Use `findDocumentById` para procurar pelo identificador exclusivo do documento.

```java
Context context = getContext(); try {
  String collectionName = "people"; JSONStoreQueryPart queryPart = new JSONStoreQueryPart(); // fuzzy search LIKE queryPart.addLike("name", name); JSONStoreQueryParts query = new JSONStoreQueryParts(); query.addQueryPart(queryPart); JSONStoreFindOptions options = new JSONStoreFindOptions(); // returns a maximum of 10 documents, default: returns every document options.setLimit(10); JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName); List<JSONObject> results = collection.findDocuments(query, options); // handle success } catch(JSONStoreException e) {
  // handle failure }
```
{: codeblock}

### Substituir
{: #replace }
Use `replaceDocument` para modificar documentos dentro de uma coleção. O campo usado para executar a substituição é `_id`, o identificador exclusivo do documento.

```java
Context context = getContext(); try {
  String collectionName = "people"; JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName); JSONStoreReplaceOptions options = new JSONStoreReplaceOptions(); // mark data as dirty options.setMarkDirty(true); JSONStore replacement = new JSONObject("{_id: 1, json: {age: 23, name: 'chevy'}}"); collection.replaceDocument(replacement, options); // handle success } catch(JSONStoreException e) {
  // handle failure }
```
{: codeblock}

Esse exemplo supõe que o documento `{_id: 1, json: {name: 'yoel', age: 23} }` está na coleção.

### Remover
{: #remove }
Use `removeDocumentById` para excluir um documento de uma coleção.
Os documentos não são apagados da coleção até que você chame `markDocumentClean`. Para obter mais informações, veja a seção **Integração do adaptador MobileFirst**.

```java
Context context = getContext(); try {
  String collectionName = "people"; JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName); JSONStoreRemoveOptions options = new JSONStoreRemoveOptions(); // Mark data as dirty options.setMarkDirty(true); collection.removeDocumentById(1, options); // handle success } catch(JSONStoreException e) {
  // handle failure }
```
{: codeblock}

### Remover Coleção
{: #remove-collection }
Use `removeCollection` para excluir todos os documentos armazenados dentro de uma coleção. Essa operação é semelhante ao descarte de uma tabela em termos de banco de dados.

```java
Context context = getContext(); try {
  String collectionName = "people"; JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName); collection.removeCollection(); // handle success } catch(JSONStoreException e) {
  // handle failure }
```
{: codeblock}

### Destruir
{: #destroy }
Use `destroy` para remover os dados a seguir:

* Todos os documentos
* Todas as coleções
* Todos os armazenamentos - Consulte **Suporte a múltiplos usuários** posteriormente neste tutorial
* Todos os metadados de JSONStore e artefatos de segurança - Consulte **Segurança** posteriormente neste tutorial

```java
Context context = getContext(); try {
  WLJSONStore.getInstance(context).destroy(); // handle success } catch(JSONStoreException e) {
  // handle failure }
```
{: codeblock}

## Uso avançado
{: #advanced-usage }
### Segurança
{: #security }
É possível proteger todas as coleções de um armazenamento, passando um objeto `JSONStoreInitOptions` com uma senha para a função `openCollections`. Se nenhuma senha for passada, os documentos de todas as coleções no armazenamento não serão criptografados.

Alguns metadados de segurança são armazenados nas preferências compartilhadas (Android).  
O armazenamento é criptografado com uma chave Padrão de Criptografia Avançado (AES) de 256 bits. Todas as chaves são potencializadas com a Password-Based Key Derivation Function 2 (PBKDF2).

Use `closeAll` para bloquear o acesso a todas as coleções até que você chame `openCollections` novamente. Se você considerar `openCollections` como uma função de login, será possível considerar `closeAll` como a função de logout correspondente.

Use `changePassword` para mudar a senha.

```java
Context context = getContext(); try {
  JSONStoreCollection people = new JSONStoreCollection("people"); people.setSearchField("name", SearchFieldType.STRING); people.setSearchField("age", SearchFieldType.INTEGER); List<JSONStoreCollection> collections = new LinkedList<JSONStoreCollection>(); collections.add(people); JSONStoreInitOptions options = new JSONStoreInitOptions(); options.setPassword("123"); WLJSONStore.getInstance(context).openCollections(collections, options); // handle success } catch(JSONStoreException e) {
  // handle failure }
```
{: codeblock}

#### Suporte a múltiplos usuários
{: #multiple-user-support }
É possível criar muitos armazenamentos que consistem em diferentes coleções em um único aplicativo MobileFirst. A função `openCollections` pode tomar um objeto de opções com um nome de usuário. Se nenhum nome de usuário for fornecido, o nome do usuário padrão será "**jsonstore**".

```java
Context context = getContext(); try {
  JSONStoreCollection people = new JSONStoreCollection("people"); people.setSearchField("name", SearchFieldType.STRING); people.setSearchField("age", SearchFieldType.INTEGER); List<JSONStoreCollection> collections = new LinkedList<JSONStoreCollection>(); collections.add(people); JSONStoreInitOptions options = new JSONStoreInitOptions(); options.setUsername("yoel"); WLJSONStore.getInstance(context).openCollections(collections, options); // handle success } catch(JSONStoreException e) {
  // handle failure }
```
{: codeblock}

#### Integração de adaptador MobileFirst
{: #mobilefirst-adapter-integration }
Esta seção presume que você esteja familiarizado com os adaptadores. A Integração do Adaptador é opcional e fornece maneiras de enviar dados de uma coleção para um adaptador e obter dados de um adaptador em uma coleção.
Se você precisar de mais flexibilidade, também será possível atingir esses objetivos usando as funções como `WLResourceRequest` ou sua própria instância de um `HttpClient`.

#### Implementação do adaptador
{: #adapter-implementation }
Crie um adaptador e dê a ele o nome "**JSONStoreAdapter**". Defina seus procedimentos `addPerson`, `getPeople`, `pushPeople`, `removePerson` e `replacePerson`.

```javascript
function getPeople() {
	var data = { peopleList : [{name: 'chevy', age: 23}, {name: 'yoel', age: 23}] }; WL.Logger.debug('Adapter: people, procedure: getPeople called.'); WL.Logger.debug('Sending data: ' + JSON.stringify(data)); return data; } function pushPeople(data) {
	WL.Logger.debug('Adapter: people, procedure: pushPeople called.'); WL.Logger.debug('Got data from JSONStore to ADD: ' + data); return; } function addPerson(data) {
	WL.Logger.debug('Adapter: people, procedure: addPerson called.'); WL.Logger.debug('Got data from JSONStore to ADD: ' + data); return; } function removePerson(data) {
	WL.Logger.debug('Adapter: people, procedure: removePerson called.'); WL.Logger.debug('Got data from JSONStore to REMOVE: ' + data); return; } function replacePerson(data) {
	WL.Logger.debug('Adapter: people, procedure: replacePerson called.'); WL.Logger.debug('Got data from JSONStore to REPLACE: ' + data); return; }
```
{: codeblock}

#### Carregar dados do Adaptador MobileFirst
{: #load-data-from-mobilefirst-adapter }
Para carregar dados de um adaptador, use `WLResourceRequest`.

```java
WLResponseListener responseListener = new WLResponseListener() {
  @Override public void onFailure(final WLFailResponse response) {
    // handle failure }
  @Override public void onSuccess(WLResponse response) {
    try {
      JSONArray loadedDocuments = response.getResponseJSON().getJSONArray("peopleList"); } catch(Exception e) {
      // error decoding JSON data }
  }
};

try {
  WLResourceRequest request = new WLResourceRequest(new URI("/adapters/JSONStoreAdapter/getPeople"), WLResourceRequest.GET);
  request.send(responseListener);
} catch (URISyntaxException e) {
  // handle error }
```
{: codeblock}

#### Obtenção de push necessária (Documentos modificados e não salvos)
{: #get-push-required-dirty-documents }
Chamar `findAllDirtyDocuments` retorna uma matriz de assim chamados "documentos modificados e não salvos", que são documentos que têm modificações locais que não existem no sistema back-end.

```java
Context context = getContext(); try {
  String collectionName = "people"; JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName); List<JSONObject> dirtyDocs = collection.findAllDirtyDocuments(); // handle success } catch(JSONStoreException e) {
  // handle failure }
```
{: codeblock}

Para evitar que o JSONStore marque os documentos como "modificados e não salvos", passe a opção `options.setMarkDirty(false)` para `add`, `replace` e `remove`.

#### Enviar mudanças por push
{: #push-changes }
Para enviar as mudanças por push para um adaptador, chame `findAllDirtyDocuments` para obter uma lista de documentos com modificações e, em seguida, use `WLResourceRequest`. Depois que os dados forem enviados e uma resposta bem-sucedida for recebida, certifique-se de chamar `markDocumentsClean`.

```java
WLResponseListener responseListener = new WLResponseListener() {
  @Override public void onFailure(final WLFailResponse response) {
    // handle failure }
  @Override public void onSuccess(WLResponse response) {
    // handle success }
}; Context context = getContext();

try {
  String collectionName = "people"; JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName); List<JSONObject> dirtyDocuments = people.findAllDirtyDocuments();

  JSONObject payload = new JSONObject(); payload.put("people", dirtyDocuments);

  WLResourceRequest request = new WLResourceRequest(new URI("/adapters/JSONStoreAdapter/pushPeople"), WLResourceRequest.POST);
  request.send(payload, responseListener);
} catch(JSONStoreException e) {
  // handle failure } catch (URISyntaxException e) {
  // handle error }
```
{: codeblock}

## Aplicativo de Amostra
{: #sample-application }
O projeto `JSONStoreAndroid` contém um aplicativo Android nativo que usa a API de JSONStore.
Um projeto maven do adaptador JavaScript está incluído.

![Imagem do aplicativo de amostra](images/android-native-screen.jpg)

[Clique para fazer download](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreAndroid) do projeto Android Nativo.  
[Clique para fazer download](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreAdapter/tree/release80) do projeto Maven do adaptador.  

### Uso de Amostra
{: #sample-usage }
Siga o arquivo LEIA-ME.md de amostra para obter instruções.
