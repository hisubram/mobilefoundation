---

copyright:
  years: 2018, 2019
lastupdated: "2018-11-23"

keywords: synchronization of data, sync with offline storage, jsonstore sync

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Sincronizar dados com uma origem de dados
{: #sync_data_with_datasource}

É possível sincronizar automaticamente os dados entre uma coleção do JSONStore em um dispositivo com qualquer banco de dados CouchDB remoto, incluindo o [Cloudant®](https://www.ibm.com/in-en/marketplace/database-management).

## Configurar a sincronização entre JSONStore e Cloudant
{: #set_up_sync}

Para configurar a sincronização automática entre JSONStore e Cloudant, conclua as etapas a seguir:

1. Defina a **Política de sincronização** em seu app móvel.
2. Implemente o **Adaptador de sincronização** no Mobile Foundation.

### Definindo a política de sincronização
{: #define_sync_policy}

O método de sincronização entre uma coleção do JSONStore e um banco de dados Cloudant é definido pela **Política de sincronização**. Especifique a **Política de sincronização** em seu app para cada coleção.
Uma coleção do JSONStore deve ser inicializada com um campo **Política de sincronização**. A **Política de sincronização** pode ser uma das três políticas a seguir:

* `SYNC_DOWNSTREAM`
  Use a política `SYNC_DOWNSTREAM` quando desejar fazer download de dados do Cloudant para a coleção do JSONStore. Essa política é geralmente usada para dados estáticos que são necessários para armazenamento off-line. Por exemplo, lista de preços de itens em um catálogo. Toda vez que a coleção é inicializada no dispositivo, os dados são atualizados no banco de dados Cloudant remoto. Embora o banco de dados inteiro seja transferido por download pela primeira vez, as atualizações a seguir transferem por download somente o delta, consistindo nas mudanças feitas no banco de dados remoto.
  **Uso: **

  *Android*
  ```java
  initOptions.setSyncPolicy(JSONStoreSyncPolicy.SYNC_DOWNSTREAM);
  ```

  *iOS*
  ```objc
  openOptions.syncPolicy = SYNC_DOWNSTREAM;
  ```

  *Cordova*
  ```javascript
  collection.sync = {
    syncPolicy:WL.JSONStore.syncOptions.SYNC_DOWNSTREAM
  }
  ```

* `SYNC_UPSTREAM`
  Use essa política quando desejar enviar por push dados locais para um banco de dados Cloudant. Por exemplo, o upload de dados de vendas capturados off-line para um banco de dados Cloudant. Quando uma coleção é definida com a política `SYNC_UPSTREAM`, quaisquer novos registros incluídos na coleção criam um novo registro no Cloudant. Da mesma forma, qualquer documento modificado na coleção no dispositivo modificará o documento no Cloudant e os documentos excluídos na coleção também serão excluídos do banco de dados Cloudant.
  **Uso: **

  *Android*
  ```java
  initOptions.setSyncPolicy(JSONStoreSyncPolicy.SYNC_UPSTREAM);
  ```

  *iOS*
  ```objc
  openOptions.syncPolicy = SYNC_UPSTREAM;
  ```

  *Cordova*
  ```javascript
  collection.sync = {
    syncPolicy:WL.JSONStore.syncOptions.SYNC_UPSTREAM
  }
  ```

* `SYNC_NONE`
  `SYNC_NONE` é a política padrão. Escolha essa política para que a sincronização não ocorra.

A **Política de sincronização** é atribuída a uma coleção do JSONStore. Se uma coleção for inicializada com uma determinada **Política de sincronização**, ela não deverá ser mudada. A modificação da **Política de sincronização** pode levar a resultados indesejáveis.

### Implementando o adaptador de sincronização
{: #deploy_sync_adapter}

`syncAdapterPath`
Essa configuração toma o nome do adaptador que é implementado.

**Uso: **

*Android*
 ```java
 initOptions.syncAdapterPath = "JSONStoreCloudantSync"; //Here "JSONStoreCloudantSync" is the name of the adapter.
 ```

*iOS*
 ```objc
  openOptions.syncAdapterPath = @"JSONStoreCloudantSync";
 ```

*Cordova ou Ionic*
 ```javascript
  collection.sync = {
  syncAdapterPath:"JSONStoreCloudantSync"
  }
 ```

* Faça download do adaptador `JSONStoreSync` [aqui](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreCloudantSync/), configure as credenciais do Cloudant no caminho `src/main/adapter-resources/adapter.xml` e implemente-o em seu servidor Mobile Foundation.
* Configure as credenciais para o banco de dados Cloudant de back-end no Mobile Foundation Operations Console.

### Executando a operação de sincronização manualmente
{: #performing_sync_manual}

Se uma sincronização de envio de dados ou de recebimento de dados precisar ser executada a qualquer momento após a inicialização explicitamente, a API a seguir poderá ser usada:

`sync()`

Essa API executará uma sincronização de recebimento de dados se a coleção de chamada tiver uma política de sincronização configurada como `SYNC_DOWNSTREAM`. Se a política de sincronização estiver configurada como `SYNC_UPSTREAM`, uma sincronização de envio de dados do JSONStore para o banco de dados Cloudant será executada. A sincronização é executada para documentos que foram incluídos, excluídos ou substituídos.

**Uso: **

*Android*
 ```java
 WLJSONStore.getInstance(context).getCollectionByName(collection_name).sync();
 ```

*iOS*
 ```objc
  collection.sync(); //Here collection is the JSONStore collection object that was initialized
 ```

*Cordova*
 ```javascript
  WL.JSONStore.get(collectionName).sync();
 ```
