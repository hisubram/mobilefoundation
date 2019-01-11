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

#	JSONStore em aplicativos Cordova
{: #cordova_jsonstore}

## Pré-requisitos
{: #prerequisites }
* Ler a [visão geral de JSONStore](jsonstore.html).
* Certifique-se de que o MobileFirst Cordova SDK tenha sido incluído no projeto. Siga o tutorial [Incluindo o Mobile Foundation SDK em aplicativos Cordova ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/cordova/){: new_window}.

## Incluindo JSONStore
{: #adding-jsonstore }
Para incluir o plug-in JSONStore em seu aplicativo Cordova:

1. Abra uma janela **Linha de comandos** e navegue para a pasta do projeto Cordova.
2. Execute o comando: `cordova plugin add cordova-plugin-mfp-jsonstore`.

![Incluir recurso do JSONStore](images/jsonstore-add-plugin.png)

## Uso básico
{: #basic-usage }
### Inicializar
{: #initialize }
Use `init` para iniciar uma ou mais coleções do JSONStore.  

Iniciar ou provisionar uma coleção significa criar o armazenamento persistente que contém a coleção e os documentos, se ele não existir. Se o armazenamento persistente estiver criptografado e uma senha correta for passada, os procedimentos de segurança necessários para tornar os dados acessíveis serão executados.

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
{: codeblock}

> Para recursos opcionais que podem ser ativados no momento da inicialização, veja **Segurança**, **Suporte a múltiplos usuários** e **Integração do adaptador MobileFirst** na segunda parte deste tutorial.

### Obter
{: #get }
Use `get` para criar um acessador para a coleção. Deve-se chamar `init` antes de chamar get, caso contrário, o resultado de `get` será indefinido.

```javascript
var collectionName = 'people'; var people = WL.JSONStore.get(collectionName);
```
{: codeblock}

A variável `people` pode agora ser usada para executar operações na coleção `people`, como `add`, `find` e `replace`.

### Incluir
{: #add }
Use `add` para armazenar dados como documentos dentro de uma coleção

```javascript
var collectionName = 'people'; var options = {}; var data = {name: 'yoel', age: 23};

WL.JSONStore.get(collectionName).add(data, options).then(function () {
    // handle success }).fail(function (error) {
    // handle failure });
```
{: codeblock}

### Localizar
{: #find }
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
{: codeblock}

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

### Substituir
{: #replace }
Use `replace` para modificar documentos dentro de uma coleção. O campo usado para executar a substituição é `_id`, o identificador exclusivo do documento.

```javascript
var document = {
  _id: 1, json: {name: 'chevy', age: 23}
}; var collectionName = 'people'; var options = {};

WL.JSONStore.get(collectionName).replace(document, options).then(function (numberOfDocsReplaced) {
    // handle success }).fail(function (error) {
    // handle failure });
```
{: codeblock}

Esse exemplo supõe que o documento `{_id: 1, json: {name: 'yoel', age: 23} }` está na coleção.

### Remover
{: #remove }
Use `remove` para excluir um documento de uma coleção.  
Os documentos não são apagados da coleção até que você chame push.  

> Para obter mais informações, veja a seção **Integração do adaptador MobileFirst** posteriormente neste tutorial

```javascript
var query = {_id: 1}; var collectionName = 'people'; var options = {exact: true}; WL.JSONStore.get(collectionName).remove(query, options).then(function (numberOfDocsRemoved) {
    // handle success }).fail(function (error) {
    // handle failure });
```
{: codeblock}

### Remover Coleção
{: #remove-collection }
Use `removeCollection` para excluir todos os documentos armazenados dentro de uma coleção. Essa operação é semelhante ao descarte de uma tabela em termos de banco de dados.

```javascript
var collectionName = 'people';
WL.JSONStore.get(collectionName).removeCollection().then(function (removeCollectionReturnCode) {
    // handle success }).fail(function (error) {
    // handle failure });
```
{: codeblock}

## Uso avançado
{: #advanced-usage }
### Destruir
{: #destory }
Use `destroy` para remover os dados a seguir:

* Todos os documentos
* Todas as coleções
* Todos os armazenamentos (consulte "**Suporte a múltiplos usuários**" posteriormente neste tutorial)
* Todos os metadados de JSONStore e artefatos de segurança (consulte "**Segurança**" posteriormente neste tutorial)

```javascript
var collectionName = 'people';
WL.JSONStore.destroy().then(function () {
    // handle success }).fail(function (error) {
    // handle failure });
```
{: codeblock}

### Segurança
{: #security }
É possível proteger todas as coleções de um armazenamento, passando uma senha para a função `init`. Se nenhuma senha for passada, os documentos de todas as coleções no armazenamento não serão criptografados.

A criptografia de dados está disponível somente em ambientes Android, iOS, Windows 8.1 Universal e Windows 10 UWP.  
Alguns metadados de segurança são armazenados no *keychain* (iOS), em *preferências compartilhadas* (Android) ou no *bloqueador de credenciais* (Windows 8.1).  
O armazenamento é criptografado com uma chave Padrão de Criptografia Avançado (AES) de 256 bits. Todas as chaves são potencializadas com a Password-Based Key Derivation Function 2 (PBKDF2).

Use `closeAll` para bloquear o acesso a todas as coleções até chamar `init` novamente. Se você pensa em `init` como uma função de login, é possível pensar em `closeAll` como a função de logout correspondente. Use `changePassword` para mudar a senha.

```javascript
var collections = {
  people : {
    searchFields: {name: 'string'}
  }
};
var options = {password: '123'};
WL.JSONStore.init(collections, options).then(function () {
    // handle success }).fail(function (error) {
    // handle failure });
```
{: codeblock}

#### Encryption
{: #encryption }
*Somente iOS*. Por padrão, o MobileFirst Cordova SDK para iOS depende de APIs fornecidas pelo iOS para criptografia. Se você preferir substituir isso por OpenSSL:

1. Inclua o plug-in cordova-plugin-mfp-encrypt-utils: `cordova plugin add cordova-plugin-mfp-encrypt-utils`.
2. Na lógica aplicativa, use: `WL.SecurityUtils.enableNativeEncryption(false)` para ativar a opção OpenSSL.

### Suporte a múltiplos usuários
{: #multiple-user-support }
É possível criar múltiplos armazenamentos contendo diferentes coleções em um único aplicativo MobileFirst. A função `init` pode tomar um objeto de opções com um nome de usuário. Se nenhum nome de usuário for fornecido, o nome do usuário padrão será **jsonstore**.

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

### Integração de adaptador MobileFirst
{: #mobilefirst-adapter-integration }
Esta seção presume que você esteja familiarizado com os Adaptadores.  

A Integração do Adaptador é opcional e fornece maneiras de enviar dados de uma coleção para um adaptador e obter dados de um adaptador em uma coleção.  
Será possível atingir esses objetivos usando `WLResourceRequest` ou `jQuery.ajax` se você precisar de mais flexibilidade.

### Implementação do adaptador
{: #adapter-implementation }
Crie um adaptador e dê a ele o nome "**JSONStoreAdapter**".  
Defina seus procedimentos `addPerson`, `getPeople`, `pushPeople`, `removePerson` e `replacePerson`.

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
{: #load-data-from-an-adapter }
Para carregar dados de um adaptador, use `WLResourceRequest`.

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
{: codeblock}

#### Obtenção de push necessária (Documentos modificados e não salvos)
{: #get-push-required-dirty-documents }
Chamar `getPushRequired` retorna uma matriz de assim chamados *"documentos modificados e não salvos"*, que são documentos que têm modificações locais que não existem no sistema back-end. Esses documentos são enviados para o adaptador quando `push` é chamado.

```javascript
var collectionName = 'people';
WL.JSONStore.get(collectionName).getPushRequired().then(function (dirtyDocuments) {
    // handle success }).fail(function (error) {
  // handle failure });
```
{: codeblock}

Para evitar que o JSONStore marque os documentos como "modificados e não salvos", passe a opção `{markDirty:false}` para `add`, `replace` e `remove`

Também é possível usar a API `getAllDirty` para recuperar os documentos modificados e não salvos:

```javascript
WL.JSONStore.get(collectionName).getAllDirty()
.then(function (dirtyDocuments) {
    //handle success }).fail(function (errorObject) {
    // handle failure });
```
{: codeblock}

#### Enviar mudanças por push
{: #push }
Para enviar as mudanças por push para um adaptador, chame `getAllDirty` para obter uma lista de documentos com modificações e, em seguida, use `WLResourceRequest`. Depois que os dados são enviados e uma resposta bem-sucedida é recebida, certifique-se de chamar `markClean`.

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

### Aprimorar
{: #enhance }
Use `enhance` para ampliar a API principal para atender às suas necessidades, incluindo funções em um protótipo de coleção.
Este exemplo (o fragmento de código a seguir) mostra como usar `enhance` para incluir a função `getValue` que funciona na coleção `keyvalue`. Ele toma `key` (sequência) como seu único parâmetro e retorna um único resultado.

```javascript
var collectionName = 'keyvalue';
WL.JSONStore.get(collectionName).enhance('getValue', function (key) {
    var deferred = $.Deferred(); var collection = this; //Do an exact search for the key collection.find({key: key}, {exact:true, limit: 1}).then(deferred.resolve, deferred.reject); return deferred.promise(); });

//Usage: var key = 'myKey'; WL.JSONStore.get(collectionName).getValue(key).then(function (result) {
    // handle success // result contains an array of documents with the results from the find }).fail(function () {
    // handle failure });
```
{: codeblock}

## Aplicativo de Amostra
{: #sample-application }
O projeto JSONStoreSwift contém um aplicativo Cordova que utiliza o conjunto de APIs de JSONStore.  
Um projeto Maven do adaptador JavaScript está incluído.

![Aplicativo de amostra do JSONStore](images/jsonstore-cordova.png)

[Clique para fazer download](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreCordova/tree/release80) do projeto Cordova.  
[Clique para fazer download](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreAdapter/tree/release80) do projeto Maven do adaptador.  

### Uso de Amostra
{: #sample-usage }
Siga o arquivo LEIA-ME.md de amostra para obter instruções.
