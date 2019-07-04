---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

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

# Synchronisation des données avec une source de données
{: #sync_data_with_datasource}

Vous pouvez automatiquement synchroniser des données entre une collection JSONStore sur un appareil et toute base de données CouchDB distante, y compris [Cloudant®](https://www.ibm.com/in-en/marketplace/database-management).

## Configuration de la synchronisation entre JSONStore et Cloudant
{: #set_up_sync}

Pour configurer la synchronisation automatique entre JSONStore et Cloudant, procédez comme suit :

1. Définissez la **stratégie de synchronisation** dans votre application mobile.
2. Déployez l'**adaptateur de synchronisation** sur Mobile Foundation.

### Définition de la stratégie de synchronisation
{: #define_sync_policy}

La méthode de synchronisation entre une collection JSONStore et une base de données Cloudant est définie par la **stratégie de synchronisation**. Spécifiez la **stratégie de synchronisation** dans votre application pour chaque collection.
Une collection JSONStore doit être initialisée avec une zone de **stratégie de synchronisation**. La **stratégie de synchronisation** peut être l'une des trois stratégies suivantes :

1. `SYNC_DOWNSTREAM`
  Utilisez la stratégie `SYNC_DOWNSTREAM` lorsque vous souhaitez télécharger des données de Cloudant vers la collection JSONStore. Cette stratégie est généralement utilisée pour les données statiques requises pour le stockage hors connexion. Par exemple, la liste de prix des articles d'un catalogue. Chaque fois que la collection est initialisée sur l'appareil, les données sont actualisées à partir de la base de données Cloudant distante. Bien que la base de données entière soit téléchargée pour la première fois, les actualisations suivantes ne téléchargent que le delta, comprenant les modifications apportées à la base de données distante.
  
Examinez l'utilisation suivante :

   * Android :
  
   ```java
   initOptions.setSyncPolicy(JSONStoreSyncPolicy.SYNC_DOWNSTREAM);
   ```

   * iOS : 
  
   ```objc
   openOptions.syncPolicy = SYNC_DOWNSTREAM;
   ```

   * Cordova : 
  
   ```javascript
   collection.sync = {
     syncPolicy:WL.JSONStore.syncOptions.SYNC_DOWNSTREAM
  }
   ```

2. `SYNC_UPSTREAM`
  Utilisez cette stratégie lorsque vous souhaitez transférer (push) des données locales vers une base de données Cloudant. Par exemple, le téléchargement de données de vente capturées hors ligne dans une base de données Cloudant. Lorsqu'une collection est définie avec la stratégie `SYNC_UPSTREAM`, tout nouvel enregistrement ajouté à la collection crée un enregistrement dans Cloudant. De même, tout document modifié dans la collection sur l'appareil modifie le document sur Cloudant et les documents supprimés dans la collection sont également supprimés de la base de données Cloudant.

Examinez l'utilisation suivante :

   * Android :
   ```java
   initOptions.setSyncPolicy(JSONStoreSyncPolicy.SYNC_UPSTREAM);
   ```

   * iOS :
   ```objc
   openOptions.syncPolicy = SYNC_UPSTREAM;
   ```

   * Cordova :
   ```javascript
   collection.sync = {
     syncPolicy:WL.JSONStore.syncOptions.SYNC_UPSTREAM
  }
   ```

3. `SYNC_NONE`
  `SYNC_NONE` est la stratégie par défaut. Choisissez cette stratégie pour que la synchronisation n'ait pas lieu.

La **stratégie de synchronisation** est attribuée à une collection JSONStore. Si une collection est initialisée avec une **stratégie de synchronisation**, elle ne doit pas être modifiée. La modification de la **stratégie de synchronisation** peut entraîner des résultats indésirables.

### Déploiement de l'adaptateur de synchronisation
{: #deploy_sync_adapter}

`syncAdapterPath`
Cette configuration prend le nom de l'adaptateur déployé.

Examinez l'utilisation suivante :

   * Android :
   ```java
   initOptions.syncAdapterPath = "JSONStoreCloudantSync"; //Here "JSONStoreCloudantSync" is the name of the adapter.
   ```

   * iOS :
   ```objc
    openOptions.syncAdapterPath = @"JSONStoreCloudantSync";
   ```

   * Cordova ou Ionic :
   ```javascript
    collection.sync = {
    syncAdapterPath:"JSONStoreCloudantSync"
  }
   ```

Téléchargez l'adaptateur `JSONStoreSync` à partir d'[ici](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreCloudantSync/), configurez les données d'identification Cloudant dans le chemin `src/main/adapter-resources/adapter.xml` et déployez-les sur votre serveur Mobile Foundation.

Configurez les données d'identification pour la base de données Cloudant de back end dans la console Mobile Foundation Operations.

### Exécution manuelle de l'opération de synchronisation
{: #performing_sync_manual}

Si une synchronisation en amont ou en aval doit être effectuée explicitement à tout moment après l'initialisation, l'API suivante peut être utilisée :

`sync()`

Cette API effectue une synchronisation en aval si la collection appelante comporte une stratégie de synchronisation définie sur `SYNC_DOWNSTREAM`. Si la stratégie de synchronisation est définie sur `SYNC_UPSTREAM`, une synchronisation en amont de la base de données JSONStore vers Cloudant est effectuée. La synchronisation est effectuée pour les documents ajoutés, supprimés ou remplacés.

Examinez l'utilisation suivante : 

  * Android :
 ```java
 WLJSONStore.getInstance(context).getCollectionByName(collection_name).sync();
 ```

  * iOS :
 ```objc
  collection.sync(); //Here collection is the JSONStore collection object that was initialized
 ```

  * Cordova :
 ```javascript
  WL.JSONStore.get(collectionName).sync();
 ```
