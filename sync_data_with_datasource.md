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

# Sync data with a datasource
{: #sync_data_with_datasource}

You can automatically synchronize data between a JSONStore collection on a device with any remote CouchDB database, including [Cloudant®](https://www.ibm.com/in-en/marketplace/database-management).

## Set up the synchronization between JSONStore and Cloudant
{: #set_up_sync}

To set up automatic synchronization between JSONStore and Cloudant complete the following steps:

1. Define the **Sync Policy** on your mobile app.
2. Deploy the **Sync Adapter** on Mobile Foundation.

### Defining the Sync Policy
{: #define_sync_policy}

The method of synchronization between a JSONStore collection and a Cloudant database is defined by the **Sync Policy**. Specify the **Sync Policy** in your app for each collection.
A JSONStore collection has to be initialized with a **Sync Policy** field. **Sync Policy** can be one of the following three policies:

1. `SYNC_DOWNSTREAM`
  Use the `SYNC_DOWNSTREAM` policy when you want to download data from Cloudant to the JSONStore collection. This policy is typically used for static data that is required for offline storage. For example, price list of items in a catalog. Each time the collection is initialized on the device, data is refreshed from the remote Cloudant database. While the entire database is downloaded for the first time, following refreshes would download only delta, consisting of the changes made on the remote database.
  
Review the following usage:

   * Android:
  
   ```java
   initOptions.setSyncPolicy(JSONStoreSyncPolicy.SYNC_DOWNSTREAM);
   ```

   * iOS: 
  
   ```objc
   openOptions.syncPolicy = SYNC_DOWNSTREAM;
   ```

   * Cordova: 
  
   ```javascript
   collection.sync = {
     syncPolicy:WL.JSONStore.syncOptions.SYNC_DOWNSTREAM
   }
   ```

2. `SYNC_UPSTREAM`
  Use this policy when you want to push local data to a Cloudant database. For example, uploading of sales data captured offline to a Cloudant database. When a collection is defined with the `SYNC_UPSTREAM` policy, any new records added to the collection creates a new record in Cloudant. Similarly, any document modified in the collection on the device will modify the document on Cloudant and documents deleted in the collection will also be deleted from the Cloudant database.

Review the following usage:

   * Android:
   ```java
   initOptions.setSyncPolicy(JSONStoreSyncPolicy.SYNC_UPSTREAM);
   ```

   * iOS:
   ```objc
   openOptions.syncPolicy = SYNC_UPSTREAM;
   ```

   * Cordova:
   ```javascript
   collection.sync = {
     syncPolicy:WL.JSONStore.syncOptions.SYNC_UPSTREAM
   }
   ```

3. `SYNC_NONE`
  `SYNC_NONE` is the default policy. Choose this policy for synchronization to not take place.

The **Sync Policy** is attributed to a JSONStore collection. If a collection is initialized with a particular **Sync Policy**, it shouldn't be changed. Modifying the **Sync Policy** can lead to undesirable results.

### Deploying the sync adapter
{: #deploy_sync_adapter}

`syncAdapterPath`
This configuration takes the adapter name that is deployed.

Review the following usage:

   * Android:
   ```java
   initOptions.syncAdapterPath = "JSONStoreCloudantSync"; //Here "JSONStoreCloudantSync" is the name of the adapter.
   ```

   * iOS:
   ```objc
    openOptions.syncAdapterPath = @"JSONStoreCloudantSync";
   ```

   * Cordova or Ionic:
   ```javascript
    collection.sync = {
    syncAdapterPath:"JSONStoreCloudantSync"
    }
   ```

Download the `JSONStoreSync` adapter from [here](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreCloudantSync/), configure Cloudant credentials in the path `src/main/adapter-resources/adapter.xml` and deploy it in your Mobile Foundation server.

Configure the credentials to the backend Cloudant database in the Mobile Foundation Operations console.

### Performing the sync operation manually
{: #performing_sync_manual}

If an upstream or downstream sync has to be performed at any time after the initialization explicitly, the following API can be used :

`sync()`

This API performs a downstream sync if the calling collection has a sync policy set to `SYNC_DOWNSTREAM`. If the sync policy is set to `SYNC_UPSTREAM`, an upstream sync from JSONStore to Cloudant database is performed. Sync is performed for documents that were added, deleted or replaced.

Review the following usage: 

  * Android:
 ```java
 WLJSONStore.getInstance(context).getCollectionByName(collection_name).sync();
 ```

  * iOS:
 ```objc
  collection.sync(); //Here collection is the JSONStore collection object that was initialized
 ```

  * Cordova:
 ```javascript
  WL.JSONStore.get(collectionName).sync();
 ```
