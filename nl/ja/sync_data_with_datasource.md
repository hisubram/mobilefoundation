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

# データとデータ・ソースの同期
{: #sync_data_with_datasource}

デバイス上の JSONStore コレクションと、[Cloudant®](https://www.ibm.com/in-en/marketplace/database-management) を含む任意のリモート CouchDB データベースとの間で、データを自動的に同期できます。

## JSONStore と Cloudant の間の同期のセットアップ
{: #set_up_sync}

JSONStore と Cloudant の間の自動同期をセットアップするには、以下のステップを実行します。

1. モバイル・アプリケーションに**同期ポリシー**を定義します。
2. Mobile Foundation に**同期アダプター**をデプロイします。

### 同期ポリシーの定義
{: #define_sync_policy}

JSONStore コレクションと Cloudant データベースの間の同期方式は、**同期ポリシー**によって定義されます。 コレクションごとにアプリケーション内で**同期ポリシー**を指定します。
JSONStore コレクションは**同期ポリシー**のフィールドを指定して初期化される必要があります。 **同期ポリシー**は、以下の 3 つのポリシーのいずれかにできます。

1. `SYNC_DOWNSTREAM`
  Cloudant から JSONStore コレクションにデータをダウンロードする必要がある場合、`SYNC_DOWNSTREAM` ポリシーを使用します。 このポリシーは通常、オフライン・ストレージに必要な静的データに対して使用されます。 例えば、カタログ内のアイテムの価格リストなどです。 デバイス上でコレクションが初期化されるたび、データはリモート Cloudant データベースから更新されます。 初回はデータベース全体がダウンロードされますが、以降のリフレッシュでは、リモート・データベース上で行われた変更からなる差分のみがダウンロードされます。
  
次の使用法を検討してください。

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
  ローカル・データを Cloudant データベースにプッシュする必要がある場合、このポリシーを使用します。 例えば、オフラインで収集された販売データを Cloudant データベースにアップロードする場合などです。 `SYNC_UPSTREAM` ポリシーが設定されたコレクションが定義されると、コレクションへの新規レコードの追加によって、Cloudant 内に新規レコードが作成されます。 同様に、デバイス上のコレクション内でドキュメントが変更されると、Cloudant 上のドキュメントも変更されます。また、コレクション内で削除されたドキュメントは、Cloudant データベースからも削除されます。

次の使用法を検討してください。

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
  `SYNC_NONE` がデフォルト・ポリシーです。 同期を行わない場合、このポリシーを選択します。

**同期ポリシー**は、JSONStore コレクションに割り当てられます。 特定の**同期ポリシー**を指定してコレクションが初期化された後は、ポリシーを変更しないでください。 **同期ポリシー**を変更すると、望ましくない結果が生じる可能性があります。

### 同期アダプターのデプロイ
{: #deploy_sync_adapter}

`syncAdapterPath`
この構成は、デプロイされるアダプター名を取ります。

次の使用法を検討してください。

   * Android:
   ```java
   initOptions.syncAdapterPath = "JSONStoreCloudantSync"; //Here "JSONStoreCloudantSync" is the name of the adapter.
   ```

   * iOS:
   ```objc
    openOptions.syncAdapterPath = @"JSONStoreCloudantSync";
   ```

   * Cordova または Ionic:
   ```javascript
    collection.sync = {
    syncAdapterPath:"JSONStoreCloudantSync"
  }
   ```

[ここ](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreCloudantSync/)から `JSONStoreSync` アダプターをダウンロードし、パス `src/main/adapter-resources/adapter.xml` に Cloudant 資格情報を構成し、それを Mobile Foundation サーバーにデプロイします。

Mobile Foundation Operations Console で、バックエンド Cloudant データベースへの資格情報を構成します。

### 手動での同期操作の実行
{: #performing_sync_manual}

初期化後の任意の時点で、アップストリームまたはダウンストリームの同期を明示的に実行する必要がある場合は、以下の API を使用できます。

`sync()`

この API は、呼び出し元のコレクションの同期ポリシーが `SYNC_DOWNSTREAM` に設定されている場合はダウンストリームの同期が実行されます。 同期ポリシーが `SYNC_UPSTREAM` に設定されている場合は、JSONStore から Cloudant データベースへのアップストリーム同期が実行されます。 同期は、追加、削除、または置換されたドキュメントに対して実行されます。

次の使用法を検討してください。 

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
