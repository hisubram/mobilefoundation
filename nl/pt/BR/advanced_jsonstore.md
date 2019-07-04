---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

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

# JSONStore avançado
{: #advanced_jsonstore}

## Segurança no JSONStore
{: #security_jsonstore}

<!--### Cordova
{: #security_jsonstore_cordova}-->

É possível proteger todas as coleções de um armazenamento, passando uma senha para a função `init`. Se nenhuma senha for passada, os documentos de todas as coleções no armazenamento não serão criptografados.
{: cordova}

A criptografia de dados está disponível somente em ambientes Android, iOS, Windows 8.1 Universal e Windows 10 UWP.
Alguns metadados de segurança são armazenados no keychain (iOS), em preferências compartilhadas (Android) ou no bloqueador de credenciais (Windows 8.1).
O armazenamento é criptografado com uma chave Padrão de Criptografia Avançado (AES) de 256 bits. Todas as chaves são potencializadas com a Password-Based Key Derivation Function 2 (PBKDF2).
{: cordova}

Use `closeAll` para bloquear o acesso a todas as coleções até chamar `init` novamente. Se você pensa em `init` como uma função de login, é possível pensar em `closeAll` como a função de logout correspondente. Use `changePassword` para mudar a senha.
{: cordova}

A criptografia é suportada somente no iOS. Por padrão, o Mobile Foundation Cordova SDK para iOS depende de APIs fornecidas pelo iOS para criptografia. Se você preferir substituir isso por OpenSSL:
{: cordova}

* Inclua o plug-in `cordova-plugin-mfp-encrypt-utils`:
  ```bash
  cordova plugin add cordova-plugin-mfp-encrypt-utils.
  ```
* Na lógica de aplicativo, use: `WL.SecurityUtils.enableNativeEncryption(false)` para ativar a opção OpenSSL.
{: cordova}

<!--### iOS
{: #security_jsonstore_ios} -->

É possível proteger todas as coleções em um armazenamento, passando um objeto `JSONStoreOpenOptions` com uma senha para a função `openCollections`. Se nenhuma senha for passada, os documentos de todas as coleções no armazenamento não serão criptografados.
{: ios}

Alguns metadados de segurança são armazenados no keychain (iOS).
O armazenamento é criptografado com uma chave Padrão de Criptografia Avançado (AES) de 256 bits. Todas as chaves são potencializadas com a Password-Based Key Derivation Function 2 (PBKDF2).
{: ios}

Use `closeAllCollections` para bloquear o acesso a todas as coleções até chamar `openCollections` novamente. Se você pensa em `openCollections` como uma função de login, é possível pensar em `closeAllCollections` como a função de logout correspondente.
{: ios}

Use `changeCurrentPassword` para mudar a senha.
{: ios}

```swift
let collection:JSONStoreCollection = JSONStoreCollection(name: "people") collection.setSearchField("name", withType: JSONStore_String) collection.setSearchField("age", withType: JSONStore_Integer)

let options:JSONStoreOpenOptions = JSONStoreOpenOptions()
options.password = "123"

do {
  try JSONStore.sharedInstance().openCollections([collection], withOptions: options)
} catch let error as NSError {
  // handle error }
```
{: codeblock}
{: ios}

<!--### Android
{: #security_jsonstore_android} -->

É possível proteger todas as coleções de um armazenamento, passando um objeto `JSONStoreInitOptions` com uma senha para a função `openCollections`. Se nenhuma senha for passada, os documentos de todas as coleções no armazenamento não serão criptografados.
{: android}

Alguns metadados de segurança são armazenados nas preferências compartilhadas (Android).
O armazenamento é criptografado com uma chave Padrão de Criptografia Avançado (AES) de 256 bits. Todas as chaves são potencializadas com a Password-Based Key Derivation Function 2 (PBKDF2).
{: android}

Use `closeAllCollections` para bloquear o acesso a todas as coleções até chamar `openCollections` novamente. Se você pensa em `openCollections` como uma função de login, é possível pensar em `closeAllCollections` como a função de logout correspondente.
{: android}

Use `changeCurrentPassword` para mudar a senha.
{: android}

```java
Context context = getContext(); try {
  JSONStoreCollection people = new JSONStoreCollection("people"); people.setSearchField("name", SearchFieldType.STRING); people.setSearchField("age", SearchFieldType.INTEGER); List<JSONStoreCollection> collections = new LinkedList<JSONStoreCollection>(); collections.add(people); JSONStoreInitOptions options = new JSONStoreInitOptions(); options.setPassword("123"); WLJSONStore.getInstance(context).openCollections(collections, options); // handle success } catch(JSONStoreException e) {
  // handle failure }
```
{: codeblock}
{: android}

## Suporte a múltiplos usuários no JSONStore
{: #multiple_user_jsonstore}

<!--### Cordova
{: #multiple_user_jsonstore_cordova} -->

É possível criar múltiplos armazenamentos contendo diferentes coleções em um único aplicativo MobileFirst. A função `init` pode usar um objeto de opções com um nome de usuário. Se nenhum nome de usuário for fornecido, o nome do usuário padrão será *jsonstore*.
{: cordova}

```javascript
var collections = {
  people : {
    searchFields: {name: 'string'}
  }
}; var options = {username: 'yoel'}; WL.JSONStore.init(collections, options).then(function () {
    // handle success }).fail(function (error) {
    // handle failure });
```
{: codeblock}
{: cordova}

<!--### iOS
{: #multiple_user_jsonstore_ios} -->

É possível criar múltiplos armazenamentos contendo diferentes coleções em um único aplicativo MobileFirst. A função `init` pode usar um objeto de opções com um nome de usuário. Se nenhum nome de usuário for fornecido, o nome do usuário padrão será *jsonstore*.
{: ios}

```swift
let collection:JSONStoreCollection = JSONStoreCollection(name: "people") collection.setSearchField("name", withType: JSONStore_String) collection.setSearchField("age", withType: JSONStore_Integer)

let options:JSONStoreOpenOptions = JSONStoreOpenOptions()
options.username = "yoel"

do {
  try JSONStore.sharedInstance().openCollections([collection], withOptions: options)
} catch let error as NSError {
  // handle error }
```
{: codeblock}
{: ios}

<!--### Android
{: #multiple_user_jsonstore_android} -->

É possível criar múltiplos armazenamentos que contenham diferentes coleções em um único aplicativo do Mobile Foundation. A função `openCollections` pode tomar um objeto de opções com um nome de usuário. Se nenhum nome de usuário for fornecido, o nome do usuário padrão será *jsonstore*.
{: android}

```java
Context context = getContext(); try {
  JSONStoreCollection people = new JSONStoreCollection("people"); people.setSearchField("name", SearchFieldType.STRING); people.setSearchField("age", SearchFieldType.INTEGER); List<JSONStoreCollection> collections = new LinkedList<JSONStoreCollection>(); collections.add(people); JSONStoreInitOptions options = new JSONStoreInitOptions(); options.setUsername("yoel"); WLJSONStore.getInstance(context).openCollections(collections, options); // handle success } catch(JSONStoreException e) {
  // handle failure }
```
{: codeblock}
{: android}

## Integração do adaptador
{: #adapter_integration}

<!--### Cordova
{: #adapter_integration_cordova}-->

A integração do adaptador é opcional e fornece maneiras de enviar dados de uma coleção para um adaptador e de obter dados de um adaptador para uma coleção.
Será possível atingir esses objetivos usando `WLResourceRequest` ou `jQuery.ajax` se você precisar de mais flexibilidade.
{: cordova}

1. Crie um adaptador e dê a ele o nome **JSONStoreAdapter**.
2. Defina seus procedimentos `addPerson`, `getPeople`, `pushPeople`, `removePerson` e `replacePerson`.
   ```javascript
   function getPeople() {
    var data = { peopleList : [{name: 'chevy', age: 23}, {name: 'yoel', age: 23}] }; WL.Logger.debug('Adapter: people, procedure: getPeople called.'); 	WL.Logger.debug('Sending data: ' + JSON.stringify(data)); 	return data; } function pushPeople(data) {
        WL.Logger.debug('Adapter: people, procedure: pushPeople called.'); 	WL.Logger.debug('Got data from JSONStore to ADD: ' + data); 	return; } function addPerson(data) {
        WL.Logger.debug('Adapter: people, procedure: addPerson called.'); 	WL.Logger.debug('Got data from JSONStore to ADD: ' + data); 	return; } function removePerson(data) {
        WL.Logger.debug('Adapter: people, procedure: removePerson called.'); 	WL.Logger.debug('Got data from JSONStore to REMOVE: ' + data); 	return; } function replacePerson(data) {
        WL.Logger.debug('Adapter: people, procedure: replacePerson called.'); 	WL.Logger.debug('Got data from JSONStore to REPLACE: ' + data); 	return; }
   ```
   {: cordova}
3. Para carregar dados de um adaptador, use `WLResourceRequest`.
   ```javascript
   try {
     var resource = new WLResourceRequest("adapters/JSONStoreAdapter/getPeople", WLResourceRequest.GET); resource.send()
     .then(function (responseFromAdapter) {
          var data = responseFromAdapter.responseJSON.peopleList;
     },function(err){
          //handle failure
     });
    } catch (e) {
        alert("Failed to load data from adapter " + e.Messages); }
   ```
   {: cordova}   
4. Chamar `getPushRequired` retorna uma matriz de assim chamados "documentos modificados e não salvos", que são documentos que têm modificações locais que não existem no sistema back-end. Esses documentos são enviados para o adaptador quando `push` é chamado.
   ```javascript
   var collectionName = 'people';
   WL.JSONStore.get(collectionName).getPushRequired().then(function (dirtyDocuments) {
        // handle success }).fail(function (error) {
      // handle failure });
   ```
   {: codeblock}
   {: cordova}
   Para evitar que o JSONStore marque os documentos como "modificados e não salvos", passe a opção `{markDirty:false}` para `add`, `replace` e `remove`.
   {: tip}
   {: cordova}
5. Também é possível usar a API `getAllDirty` para recuperar os documentos modificados e não salvos.
   ```javascript
   WL.JSONStore.get(collectionName).getAllDirty()
    .then(function (dirtyDocuments) {
        //handle success }).fail(function (errorObject) {
        // handle failure });
   ```
   {: codeblock}
   {: cordova}
6. Para enviar as mudanças por push para um adaptador, chame `getAllDirty` para obter uma lista de documentos com modificações e, em seguida, use `WLResourceRequest`. Depois que os dados são enviados e uma resposta bem-sucedida é recebida, certifique-se de chamar `markClean`.
   ```javascript
   try {
     var collectionName = "people"; var dirtyDocs;

     WL.JSONStore.get(collectionName)
     .getAllDirty()
     .then(function (arrayOfDirtyDocuments) {
        dirtyDocs = arrayOfDirtyDocuments;

        var resource = new WLResourceRequest("adapters/JSONStoreAdapter/pushPeople", WLResourceRequest.POST); resource.setQueryParameter('params', [dirtyDocs]); return resource.send(); }).then(function (responseFromAdapter) {
            return WL.JSONStore.get(collectionName).markClean(dirtyDocs); }).then(function (res) {
          // handle success }).fail(function (errorObject) {
          // Handle failure.
        });

    } catch (e) {
        alert("Failed To Push Documents to Adapter"); }
   ```
   {: codeblock}
   {: cordova}
7. Use `enhance` para ampliar a API principal para atender às suas necessidades, incluindo funções em um protótipo de coleção. Este exemplo (o fragmento de código a seguir) mostra como usar `enhance` para incluir a função `getValue` que funciona na coleta `keyvalue`. Ele toma key (sequência) como seu único parâmetro e retorna um único resultado.
   ```javascript
   var collectionName = 'keyvalue';
    WL.JSONStore.get(collectionName).enhance('getValue', function (key) {
        var deferred = $.Deferred(); var collection = this; //Do an exact search for the key collection.find({key: key}, {exact:true, limit: 1}).then(deferred.resolve, deferred.reject); return deferred.promise(); });

    //Usage: var key = 'myKey'; WL.JSONStore.get(collectionName).getValue(key).then(function (result) {
        // handle success // result contains an array of documents with the results from the find }).fail(function () {
        // handle failure });
   ```
   {: codeblock}
   {: cordova}
8. Consulte a amostra JSONStore para o app Cordova na seção **Amostras**. Este projeto contém um aplicativo Cordova que usa o conjunto de APIs JSONStore. O projeto Maven do adaptador JavaScript pode ser transferido por download [daqui](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreAdapter/tree/release80).
{: cordova}

<!--### iOS
{: #adapter_integration_ios}-->

A integração do adaptador é opcional e fornece maneiras de enviar dados de uma coleção para um adaptador e de obter dados de um adaptador para uma coleção.
É possível atingir esses objetivos usando `WLResourceRequest`.
{: ios}

1. Crie um adaptador e nomeie-o como **Pessoas**.
2. Defina seus procedimentos `addPerson`, `getPeople`, `pushPeople`, `removePerson` e `replacePerson`.
3. Para carregar dados de um adaptador, use `WLResourceRequest`.
   ```swift
    // Start - LoadFromAdapter
    class LoadFromAdapter: NSObject, WLDelegate {
      func onSuccess(response: WLResponse!) {
        let responsePayload:NSDictionary = response.getResponseJson()
        let people:NSArray = responsePayload.objectForKey("peopleList") as! NSArray
        // handle success
      }

      func onFailure(response: WLFailResponse!) {
        // handle failure }
    }
    // End - LoadFromAdapter

    let pull = WLResourceRequest(URL: NSURL(string: "/adapters/People/getPeople"), method: "GET")

    let loadDelegate:LoadFromAdapter = LoadFromAdapter()
    pull.sendWithDelegate(loadDelegate)
   ```
   {: codeblock}
   {: ios}
4. Chamar `allDirty` retorna uma matriz de assim chamados "documentos modificados e não salvos", que são documentos que têm modificações locais que não existem no sistema back-end.
   ```swift
    let collectionName:String = "people" let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

    fazer {
      let dirtyDocs:NSArray = try collection.allDirty()
    } catch let error as NSError {
      // handle error }
   ```
   {: codeblock}
   {: ios}
   Para evitar que o JSONStore marque os documentos como "modificados e não salvos", passe a opção `{markDirty:false}` para `add`, `replace` e `remove`.
   {: tip}
5. Para enviar por push as mudanças para um adaptador, chame o `allDirty` para obter uma lista de documentos com modificações e, em seguida, use `WLResourceRequest`. Depois que os dados são enviados e uma resposta bem-sucedida é recebida, certifique-se de chamar `markDocumentsClean`.
   ```swift
    // Start - PushToAdapter
    class PushToAdapter: NSObject, WLDelegate {
      func onSuccess(response: WLResponse!) {
        // handle success
      }

      func onFailure(response: WLFailResponse!) {
        // handle failure }
    }
    // End - PushToAdapter

    let collectionName:String = "people" let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

    fazer {
      let dirtyDocs:NSArray = try collection.allDirty()
      let pushData:NSData = NSKeyedArchiver.archivedDataWithRootObject(dirtyDocs)

      let push = WLResourceRequest(URL: NSURL(string: "/adapters/People/pushPeople"), method: "POST")

      let pushDelegate:PushToAdapter = PushToAdapter()
      push.sendWithData(pushData, delegate: pushDelegate)

    } catch let error as NSError {
      // handle error }
   ```
   {: codeblock}
   {: ios}
6. Faça download do projeto do aplicativo iOS Swift nativo na seção **Amostras**. O projeto contém um aplicativo iOS Swift nativo que usa o conjunto de APIs JSONStore. O projeto Maven do adaptador JavaScript pode ser transferido por download [daqui](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreAdapter/tree/release80).
{: ios}

<!--### Android
{: #adapter_integration_android}-->

A integração do adaptador é opcional e fornece maneiras de enviar dados de uma coleção para um adaptador e de obter dados de um adaptador para uma coleção.
Será possível atingir esses objetivos usando funções como `WLResourceRequest` ou sua própria instância de um `HttpClient` se você precisar de mais flexibilidade.
{: android}

1. Crie um adaptador e dê a ele o nome **JSONStoreAdapter**.
2. Defina seus procedimentos `addPerson`, `getPeople`, `pushPeople`, `removePerson` e `replacePerson`.
3. Para carregar dados de um adaptador, use `WLResourceRequest`.
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
   {: android}
4. Chamar `findAllDirtyDocuments` retorna uma matriz de assim chamados "documentos modificados e não salvos", que são documentos que têm modificações locais que não existem no sistema back-end.
   ```java
    Context context = getContext(); try {
      String collectionName = "people"; JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName); List<JSONObject> dirtyDocs = collection.findAllDirtyDocuments(); // handle success } catch(JSONStoreException e) {
      // handle failure }
   ```
   {: codeblock}
   {: android}
   Para evitar que o JSONStore marque os documentos como "modificados e não salvos", passe a opção `options.setMarkDirty(false)` para `add`, `replace` e `remove`.
   {: tip}
   {: android}
5. Para enviar as mudanças por push para um adaptador, chame `findAllDirtyDocuments` para obter uma lista de documentos com modificações e, em seguida, use `WLResourceRequest`. Depois que os dados são enviados e uma resposta bem-sucedida é recebida, certifique-se de chamar `markDocumentsClean`.
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
   {: android}
6. Faça download do projeto do aplicativo Android nativo na seção **Amostras**. O projeto contém um aplicativo Android nativo que usa o conjunto de APIs JSONStore. O projeto Maven do adaptador JavaScript pode ser transferido por download [daqui](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreAdapter/tree/release80).
{: android}
