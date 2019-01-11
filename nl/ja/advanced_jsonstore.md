---

copyright:
  years: 2018
lastupdated: "2018-11-28"

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
{:ios: .ph data-hd-operatingsystem='iOS'}
{:android: .ph data-hd-operatingsystem='Android'}
{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# オフライン・ストレージの拡張構成 
{: #adv_configure_offline_storage}

このページでは、上のタブを使用して OS 固有の説明を参照できます。Android 固有の説明を参照するには **Java** を、Cordova 固有の説明を参照するには **Node** を、iOS 固有の説明を参照するには **Swift** を選択します。
{: note}

## JSONStore でのセキュリティー
{: #security_jsonstore} 

<!--### Cordova
{: security_jsonstore_cordova}-->

パスワードを `init` 関数に渡すことにより、ストア内のすべてのコレクションを保護することができます。 パスワードを渡さないと、ストア内のすべてのコレクションにあるドキュメントが暗号化されません。
{: javascript}

データ暗号化は、Android、iOS、Windows 8.1 Universal および Windows 10 UWP の各環境でのみ使用可能です。
一部のセキュリティー・メタデータは、キーチェーン (iOS)、共有設定 (Android) または資格情報保管ボックス (Windows 8.1) に保管されます。
ストアは 256 ビットの Advanced Encryption Standard (AES) 鍵で暗号化されます。 すべての鍵は Password-Based Key Derivation Function 2 (PBKDF2) により強化されています。
{: javascript}

`closeAll` を使用して、`init` を再度呼び出すまですべてのコレクションへのアクセスをロックします。 `init` をログイン関数と考えると、`closeAll` はそれに対応するログアウト関数と考えることができます。 `changePassword` を使用して、パスワードを変更します。
{: javascript}

暗号化は iOS でのみサポートされます。デフォルトでは、Mobile Foundation Cordova SDK for iOS は、iOS 提供の API に暗号化を依存しています。 これを OpenSSL に置換したい場合は、以下のようにします。
{: javascript}

* `cordova-plugin-mfp-encrypt-utils` プラグインを追加します。 
  ```bash
  cordova plugin add cordova-plugin-mfp-encrypt-utils
  ```
* アプリケーション・ロジックで、`WL.SecurityUtils.enableNativeEncryption(false)` を使用して OpenSSL オプションを有効にします。
{: javascript}

<!--### iOS
{: #security_jsonstore_ios} -->

`JSONStoreOpenOptions` オブジェクトとパスワードを `openCollections` 関数に渡すことにより、ストア内のすべてのコレクションを保護できます。 パスワードを渡さないと、ストア内のすべてのコレクションにあるドキュメントが暗号化されません。
{: swift}

一部のセキュリティー・メタデータはキーチェーンに保管されます (iOS)。
ストアは 256 ビットの Advanced Encryption Standard (AES) 鍵で暗号化されます。 すべての鍵は Password-Based Key Derivation Function 2 (PBKDF2) により強化されています。
{: swift}

`closeAllCollections` を使用して、`openCollections` を再度呼び出すまですべてのコレクションへのアクセスをロックします。 `openCollections` をログイン関数と考えると、`closeAllCollections` はそれに対応するログアウト関数と考えることができます。
{: ios}

`changeCurrentPassword` を使用して、パスワードを変更します。
{: swift}

```swift
let collection:JSONStoreCollection = JSONStoreCollection(name: "people")
collection.setSearchField("name", withType: JSONStore_String)
collection.setSearchField("age", withType: JSONStore_Integer)

let options:JSONStoreOpenOptions = JSONStoreOpenOptions()
options.password = "123"

do {
  try JSONStore.sharedInstance().openCollections([collection], withOptions: options)
} catch let error as NSError {
  // handle error
}
```
{: codeblock}
{: swift}

<!--### Android
{: #security_jsonstore_android} -->

`JSONStoreInitOptions` オブジェクトとパスワードを `openCollections` 関数に渡すことにより、ストア内のすべてのコレクションを保護できます。 パスワードを渡さないと、ストア内のすべてのコレクションにあるドキュメントが暗号化されません。
{: java}

一部のセキュリティー・メタデータは共有設定で保管されます (Android)。
ストアは 256 ビットの Advanced Encryption Standard (AES) 鍵で暗号化されます。 すべての鍵は Password-Based Key Derivation Function 2 (PBKDF2) により強化されています。
{: java}

`closeAllCollections` を使用して、`openCollections` を再度呼び出すまですべてのコレクションへのアクセスをロックします。 `openCollections` をログイン関数と考えると、`closeAllCollections` はそれに対応するログアウト関数と考えることができます。
{: java}

`changeCurrentPassword` を使用して、パスワードを変更します。
{: java}

```java
Context  context = getContext();
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
{: java}

## JSONStore での複数ユーザー・サポート
{: #multiple_user_jsonstore} 

<!--### Cordova
{: #multiple_user_jsonstore_cordova} -->

単一の MobileFirst アプリケーションに、異なるコレクションを含む複数のストアを作成できます。 `init` 関数はオプション・オブジェクトとユーザー名を受け取ります。 ユーザー名が指定されていない場合、デフォルトのユーザー名 *jsonstore* が使用されます。
{: javascript}

```javascript
var collections = {
  people: {
    searchFields: {name: 'string'}
  }
};
var options = {username: 'yoel'};
WL.JSONStore.init(collections, options).then(function () {
    // handle success
}).fail(function (error) {
    // handle failure
    });
```
{: codeblock}
{: javascript}

<!--### iOS
{: #multiple_user_jsonstore_ios} -->

単一の MobileFirst アプリケーションに、異なるコレクションを含む複数のストアを作成できます。 `init` 関数はオプション・オブジェクトとユーザー名を受け取ります。 ユーザー名が指定されていない場合、デフォルトのユーザー名 *jsonstore* が使用されます。
{: swift}

```swift
let collection:JSONStoreCollection = JSONStoreCollection(name: "people")
collection.setSearchField("name", withType: JSONStore_String)
collection.setSearchField("age", withType: JSONStore_Integer)

let options:JSONStoreOpenOptions = JSONStoreOpenOptions()
options.username = "yoel"

do {
  try JSONStore.sharedInstance().openCollections([collection], withOptions: options)
} catch let error as NSError {
  // handle error
}
```
{: codeblock}
{: swift}

<!--### Android
{: #multiple_user_jsonstore_android} -->

単一の Mobile Foundation アプリケーションに、異なるコレクションを含む複数のストアを作成できます。`openCollections` 関数はオプション・オブジェクトとユーザー名を受け取ります。 ユーザー名が指定されていない場合、デフォルトのユーザー名 *jsonstore* が使用されます。
{: java}

```java
Context  context = getContext();
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
{: java}

## アダプターの統合
{: #adapter_integration}

<!--### Cordova
{: #adapter_integration_cordova}-->

アダプターの統合はオプションであり、コレクションからアダプターにデータを送信する方法、およびアダプターからコレクションにデータを取得する方法を提供します。
これらの目的を実現するために、`WLResourceRequest` や、より高い柔軟性が必要な場合は `jQuery.ajax` を使用することができます。
{: javascript}

1. アダプターを作成し、**JSONStoreAdapter** という名前を付けます。
2. このアダプターのプロシージャー `addPerson`、`getPeople`、`pushPeople`、 `removePerson`、および `replacePerson` を定義します。
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
   {: javascript}
3. データをアダプターからロードするには、`WLResourceRequest` を使用します。
   ```javascript
   try {
     var resource = new WLResourceRequest("adapters/JSONStoreAdapter/getPeople", WLResourceRequest.GET);
     resource.send()
     .then(function (responseFromAdapter) {
          var data = responseFromAdapter.responseJSON.peopleList;
     },function(err){
          //handle failure
     });
    } catch (e) {
        alert("Failed to load data from adapter " + e.Messages);
}
   ```
   {: codeblock}
   {: javascript}
4. `getPushRequired` を呼び出すと、「ダーティーなドキュメント」と呼ばれる配列が返されます。これは、バックエンド・システムには存在しないローカル変更が含まれるドキュメントです。 これらのドキュメントは、`push` が呼び出されたときに、アダプターに送信されます。
   ```javascript
   var collectionName = 'people';
WL.JSONStore.get(collectionName).getPushRequired().then(function (dirtyDocuments) {
        // handle success
}).fail(function (error) {
      // handle failure
    });
   ```
   {: codeblock}
   {: javascript}
   JSONStore でドキュメントが「ダーティー」とマーキングされないようにするには、オプション `{markDirty:false}` を `add`、`replace`、および`remove` に渡します。
   {: tip} 
5. `getAllDirty` API を使用してダーティー・ドキュメントを取得することもできます。
   ```javascript
   WL.JSONStore.get(collectionName).getAllDirty()
    .then(function (dirtyDocuments) {
        // handle success
}).fail(function (errorObject) {
        // handle failure
    });
   ```
   {: javascript}
   {: codeblock}
6. 変更をアダプターにプッシュするには、`getAllDirty` を呼び出して変更が含まれるドキュメントのリストを取得し、その後 `WLResourceRequest` を使用します。 データが送信され、成功応答を受信した後、`markClean` を呼び出す必要があります。
   ```javascript
   try {
     var collectionName = "people";
     var dirtyDocs;

     WL.JSONStore.get(collectionName)
     .getAllDirty()
     .then(function (arrayOfDirtyDocuments) {
        dirtyDocs = arrayOfDirtyDocuments;

        var resource = new WLResourceRequest("adapters/JSONStoreAdapter/pushPeople", WLResourceRequest.POST);
        resource.setQueryParameter('params', [dirtyDocs]);
        return resource.send();
     }).then(function (responseFromAdapter) {
            return WL.JSONStore.get(collectionName).markClean(dirtyDocs);
     }).then(function (res) {
          //handle success
     }).fail(function (errorObject) {
          // Handle failure.
        });

    } catch (e) {
        alert("Failed To Push Documents to Adapter");
}
   ```
   {: codeblock}
   {: javascript}
7. コア API をニーズに合うように拡張するには、`enhance` を使用します。それには、関数をコレクションのプロトタイプに追加します。 この例 (下記のコード・スニペット) は、`enhance` を使用して、`keyvalue` コレクションで動作する関数 `getValue` を追加する方法を示しています。 この関数は、唯一のパラメーターとして key (ストリング) を受け取り、単一の結果を返します。
   ```javascript
   var collectionName = 'keyvalue';
WL.JSONStore.get(collectionName).enhance('getValue', function (key) {
        var deferred = $.Deferred();
    var collection = this;
    //Do an exact search for the key
    collection.find({key: key}, {exact:true, limit: 1}).then(deferred.resolve, deferred.reject);
    return deferred.promise();
});

    //Usage:
var key = 'myKey';
WL.JSONStore.get(collectionName).getValue(key).then(function (result) {
        // handle success
    // result contains an array of documents with the results from the find
}).fail(function () {
        // handle failure
    }); 
   ```
   {: codeblock}
   {: javascript}
8. **Samples** セクションから、Cordova アプリ用の JSONStore サンプルを確認します。このプロジェクトには、JSONStore API セットを使用する Cordova アプリケーションが含まれています。 JavaScript アダプター Maven プロジェクトは[ここ](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreAdapter/tree/release80)からダウンロードできます。
{: javascript}

<!--### iOS
{: #adapter_integration_ios}-->

アダプターの統合はオプションであり、コレクションからアダプターにデータを送信する方法、およびアダプターからコレクションにデータを取得する方法を提供します。
`WLResourceRequest` を使用して、これらの目的を実現することができます。
{: swift}

1. アダプターを作成し、**People** という名前を付けます。
2. このアダプターのプロシージャー `addPerson`、`getPeople`、`pushPeople`、 `removePerson`、および `replacePerson` を定義します。
   ```swift
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
   {: swift}
3. データをアダプターからロードするには、`WLResourceRequest` を使用します。
   ```swift
    // Start - LoadFromAdapter
    class LoadFromAdapter: NSObject, WLDelegate {
      func onSuccess(response: WLResponse!) {
        let responsePayload:NSDictionary = response.getResponseJson()
        let people:NSArray = responsePayload.objectForKey("peopleList") as! NSArray
        // handle success
      }

      func onFailure(response: WLFailResponse!) {
        // handle failure
      }
    }
    // End - LoadFromAdapter

    let pull = WLResourceRequest(URL: NSURL(string: "/adapters/People/getPeople"), method: "GET")

    let loadDelegate:LoadFromAdapter = LoadFromAdapter()
    pull.sendWithDelegate(loadDelegate)
   ```
   {: codeblock}
   {: swift}
4. `allDirty` を呼び出すと、「ダーティーなドキュメント」と呼ばれる配列が返されます。これは、バックエンド・システムには存在しないローカル変更が含まれるドキュメントです。
   ```swift
    let collectionName:String = "people"
    let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

    do {
      let dirtyDocs:NSArray = try collection.allDirty()
    } catch let error as NSError {
      // handle error
    }
   ```
   {: codeblock}
   {: swift}
   JSONStore でドキュメントが「ダーティー」とマーキングされないようにするには、オプション `{markDirty:false}` を `add`、`replace`、および`remove` に渡します。
   {: tip} 
5. 変更をアダプターにプッシュするには、`allDirty` を呼び出して変更が含まれるドキュメントのリストを取得し、その後 `WLResourceRequest` を使用します。 データが送信され、成功応答を受信した後、`markDocumentsClean` を呼び出す必要があります。
   ```swift
    // Start - PushToAdapter
    class PushToAdapter: NSObject, WLDelegate {
      func onSuccess(response: WLResponse!) {
        // handle success
      }

      func onFailure(response: WLFailResponse!) {
        // handle failure
      }
    }
    // End - PushToAdapter

    let collectionName:String = "people"
    let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

    do {
      let dirtyDocs:NSArray = try collection.allDirty()
      let pushData:NSData = NSKeyedArchiver.archivedDataWithRootObject(dirtyDocs)

      let push = WLResourceRequest(URL: NSURL(string: "/adapters/People/pushPeople"), method: "POST")

      let pushDelegate:PushToAdapter = PushToAdapter()
      push.sendWithData(pushData, delegate: pushDelegate)

    } catch let error as NSError {
      // handle error
    }
   ```
   {: codeblock}
   {: swift}
6. **Samples** セクションから、ネイティブ iOS Swift アプリケーション・プロジェクトをダウンロードします。プロジェクトには、JSONStore API セットを使用するネイティブ iOS Swift アプリケーションが含まれています。 JavaScript アダプター Maven プロジェクトは[ここ](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreAdapter/tree/release80)からダウンロードできます。
{: swift}

<!--### Android
{: #adapter_integration_android}-->

アダプターの統合はオプションであり、コレクションからアダプターにデータを送信する方法、およびアダプターからコレクションにデータを取得する方法を提供します。
`WLResourceRequest` などの関数を使用することで、またはより柔軟性が必要な場合は `HttpClient` の独自インスタンスを使用することで、これらの目標を達成できます。
{: java}

1. アダプターを作成し、**JSONStoreAdapter** という名前を付けます。
2. このアダプターのプロシージャー `addPerson`、`getPeople`、`pushPeople`、 `removePerson`、および `replacePerson` を定義します。
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
   {: java}
3. データをアダプターからロードするには、`WLResourceRequest` を使用します。
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
   {: java}
4. `findAllDirtyDocuments` を呼び出すと、「ダーティーなドキュメント」と呼ばれる配列が返されます。これは、バックエンド・システムには存在しないローカル変更が含まれるドキュメントです。
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
   {: java}
   JSONStore でドキュメントが「ダーティー」とマーキングされないようにするには、オプション `options.setMarkDirty(false)` を `add`、`replace`、および`remove` に渡します。
   {: tip} 
5. 変更をアダプターにプッシュするには、`findAllDirtyDocuments` を呼び出して変更が含まれるドキュメントのリストを取得し、その後 `WLResourceRequest` を使用します。 データが送信され、成功応答を受信した後、`markDocumentsClean` を呼び出す必要があります。
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
   {: java}
6. **Samples** セクションから、ネイティブ Android アプリケーション・プロジェクトをダウンロードします。プロジェクトには、JSONStore API セットを使用するネイティブ Android アプリケーションが含まれています。 JavaScript アダプター Maven プロジェクトは[ここ](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreAdapter/tree/release80)からダウンロードできます。
{: java}
