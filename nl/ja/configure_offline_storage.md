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

# オフライン・ストレージの構成
{: #configure_offline_storage}

Mobile Foundation JSONStore は、軽量なドキュメント指向のストレージ・システムを提供する、オプションのクライアント・サイド API です。 JSONStore を使用すると、JSON ドキュメントを永続的に保管できます。 JSONStore では、アプリケーションを実行しているデバイスがオフラインの時でも、アプリケーションのドキュメントを使用できます。 この永続的に常時使用できるストレージにより、例えば、使用可能なネットワーク接続がデバイスにないときでも、ドキュメントにアクセスできるため便利です。 JSONStore の概念と用語の概説については、[こちら](jsonstore.html)を参照してください。

## Cordova または Ionic アプリケーション用のオフライン・ストレージの構成
{: #configure_offline_storage_cordova}

Mobile Foundation Cordova SDK がプロジェクトに追加されていることを確認します。 

[Cordova アプリケーションへの Mobile Foundation SDK の追加 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/cordova/) のチュートリアルに従ってください。
{: tip}

### Cordova プロジェクトへの JSONStore の追加
{: #adding_jsonstore_cordova}

1. コマンド・ライン・ウィンドウを開き、Cordova プロジェクト・フォルダーにナビゲートします。
2. 次のコマンドを実行します。 
   ```bash
   cordova plugin add cordova-plugin-mfp-jsonstore
   ```

### JSONStore コレクションの初期化
{: #initialize_jsonstore_cordova}   

1 つ以上の JSONStore コレクションを開始するには `init` を使用します。
コレクションの開始またはプロビジョニングは、コレクションとドキュメントが含まれる永続ストレージを作成することを意味します (永続ストレージが存在しない場合)。 options にパスワードが渡されると、永続ストレージはそのパスワードを使用して暗号化されます。

```javascript
var collections = {
    people : {
        searchFields : {name: 'string', age: 'integer'}
    }
};

WL.JSONStore.init(collections).then(function (collections) {
    // handle success - collection.people (people's collection)
}).fail(function (error) {
    // handle failure
    });
```

### JSONStore コレクションへのアクセス機能の取得
{: #get_jsonstore_cordova} 

コレクションへのアクセス機能を作成するには、`get` を使用します。 get を呼び出す前に `init` を呼び出す必要があります。このようにしないと、`get` の結果は*不明確な* ものになります。

```javascript
var collectionName = 'people';
var people = WL.JSONStore.get(collectionName);
```
これで、変数 *people* を使用して、*people* コレクションに対して `add`、`find`、`replace` などの操作を実行できます。

### コレクションへのドキュメントの追加
{: #add_jsonstore_cordova} 

コレクション内にデータをドキュメントとして保管するには、`add` を使用します。

```javascript
var collectionName = 'people';
var options = {};
var data = {name: 'yoel', age: 23};

WL.JSONStore.get(collectionName).add(data, options).then(function () {
    // handle success
}).fail(function (error) {
    // handle failure
    });
```

### コレクション内のドキュメントの検索
{: #find_jsonstore_cordova} 

* 照会を使用してコレクション内のドキュメントを見つけるには、`find` を使用します。
* コレクション内のすべてのドキュメントを取り出すには、`findAll` を使用します。
* ドキュメントの固有 ID で検索するには、`findById` を使用します。

find のデフォルトの動作では、「ファジー」検索を実行します。

```javascript
var query = {name: 'yoel'};
var collectionName = 'people';
var options = {
  exact: false, //default
  limit: 10 // returns a maximum of 10 documents, default: return every document
};

WL.JSONStore.get(collectionName).find(query, options).then(function (results) {
    // handle success - results (array of documents found)
}).fail(function (error) {
    // handle failure
    });
```

```javascript
var age = document.getElementById("findByAge").value || '';

if(age == "" || isNaN(age)){
  alert("Please enter a valid age to find");
}
else {
  query = {age: parseInt(age, 10)};
  var options = {
    exact: true,
    limit: 10 //returns a maximum of 10 documents
  };
  WL.JSONStore.get(collectionName).find(query, options).then(function (res) {
    // handle success - results (array of documents found)
  }).fail(function (errorObject) {
    // handle failure
  });
}
```

### コレクション内のドキュメントの置換
{: #replace_jsonstore_cordova} 

コレクション内のドキュメントを変更するには、`replace` を使用します。 置換の実行に使用するフィールドは `_id` で、これはドキュメントの固有 ID です。

```javascript
var document = {
  _id: 1, json: {name: 'chevy', age: 23}
};
var collectionName = 'people';
var options = {};

WL.JSONStore.get(collectionName).replace(document, options).then(function (numberOfDocsReplaced) {
    // handle success
}).fail(function (error) {
    // handle failure
    });
```
この例では、ドキュメント `{_id: 1, json: {name: 'yoel', age: 23} }` がコレクションにあることを前提としています。


### コレクションからのドキュメントの削除
{: #remove_jsonstore_cordova} 

ドキュメントをコレクションから削除するには、`remove` を使用します。
push が呼び出されるまで、ドキュメントはコレクションから消去 されません。

```javascript
var query = {_id: 1};
var collectionName = 'people';
var options = {exact: true};
WL.JSONStore.get(collectionName).remove(query, options).then(function (numberOfDocsRemoved) {
    // handle success
}).fail(function (error) {
    // handle failure
    });
```

### コレクション全体の削除
{: #remove_collection_jsonstore_cordova} 

コレクション内に保管されているすべてのドキュメントを削除するには、 `removeCollection` を使用します。 この操作は、データベース用語における、表のドロップと似ています。

### JSONStore の破棄
{: #destroy_jsonstore_cordova} 

以下のデータを削除するには、`destroy` を使用します。
* すべてのドキュメント
* すべてのコレクション
* すべてのストア
* すべての JSONStore メタデータおよびセキュリティー成果物

## iOS アプリケーション用のオフライン・ストレージの構成
{: #configure_offline_storage_ios}

Mobile Foundation ネイティブ SDK が Xcode プロジェクトに追加されていることを確認します。 

[iOS アプリケーションへの Mobile Foundation SDK の追加 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/ios/) のチュートリアルに従ってください。
{: tip}

### iOS プロジェクトへの JSONStore の追加
{: #adding_jsonstore_ios}

1. Xcode プロジェクトのルートにある既存の `podfile` に以下を追加します。
   ```bash
   pod 'IBMMobileFirstPlatformFoundationJSONStore'
   ```
2. コマンド・ラインから Xcode プロジェクトのルートに移動し、次のコマンドを実行します。 
   ```bash
   pod install
   ``` 
3. JSONStore を使用する場合はいつでも、必ず JSONStore ヘッダーをインポートするようにしてください。
   **Objective-C**:
   ```objectivec
   #import <IBMMobileFirstPlatformFoundationJSONStore/IBMMobileFirstPlatformFoundationJSONStore.h>
   ``` 
   **Swift:**
   ```swift
   import IBMMobileFirstPlatformFoundationJSONStore
   ```   

### JSONStore コレクションを開く: iOS
{: #open_ios} 

1 つ以上の JSONStore コレクションを開くには、`openCollections` を使用します。

```swift
let collection:JSONStoreCollection = JSONStoreCollection(name: "people")

collection.setSearchField("name", withType: JSONStore_String)
collection.setSearchField("age", withType: JSONStore_Integer)

do {
  try JSONStore.sharedInstance().openCollections([collection], withOptions: nil)
} catch let error as NSError {
  // handle error
}
```

### JSONStore コレクションへのアクセス機能の取得
{: #get_jsonstore_ios} 

コレクションへのアクセス機能を作成するには、`getCollectionWithName` を使用します。 `getCollectionWithName` を呼び出す前に `openCollections` を呼び出す必要があります。

```swift
let collectionName:String = "people"
let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)
```
これで、変数 collection を使用して、`people` コレクションに対して `add`、`find`、`replace` などの操作を実行できます。

### コレクションへのドキュメントの追加
{: #add_jsonstore_ios} 

コレクション内にデータをドキュメントとして保管するには、`addData` を使用します。

```swift
let collectionName:String = "people"
    let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

let data = ["name" : "yoel", "age" : 23]

do {
  try collection.addData([data], andMarkDirty: true, withOptions: nil)
} catch let error as NSError {
  // handle error
}
```

### コレクション内のドキュメントの検索
{: #find_jsonstore_ios} 

照会を使用してコレクション内のドキュメントを見つけるには、`findWithQueryParts` を使用します。 コレクション内のすべてのドキュメントを取り出すには、`findAllWithOptions` を使用します。 ドキュメントの固有 ID で検索するには、`findWithIds` を使用します。

```swift
let collectionName:String = "people"
    let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

let options:JSONStoreQueryOptions = JSONStoreQueryOptions()
// returns a maximum of 10 documents, default: returns every document
options.limit = 10

let query:JSONStoreQueryPart = JSONStoreQueryPart()
query.searchField("name", like: "yoel")

do {
  let results:NSArray = try collection.findWithQueryParts([query], andOptions: options)
} catch let error as NSError {
  // handle error
}
```

### コレクション内のドキュメントの置換
{: #replace_jsonstore_ios} 

コレクション内のドキュメントを変更するには、`replaceDocuments` を使用します。 置換の実行に使用するフィールドは `_id` で、これはドキュメントの固有 ID です。

```swift
let collectionName:String = "people"
    let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

var document:Dictionary<String,AnyObject> = Dictionary()
document["name"] = "chevy"
document["age"] = 23

var replacement:Dictionary<String, AnyObject> = Dictionary()
replacement["_id"] = 1
replacement["json"] = document

do {
  try collection.replaceDocuments([replacement], andMarkDirty: true)
} catch let error as NSError {
  // handle error
}
```
この例では、ドキュメント `{_id: 1, json: {name: 'yoel', age: 23} }` がコレクションにあることを前提としています。

### コレクションからのドキュメントの削除
{: #remove_jsonstore_ios} 

ドキュメントをコレクションから削除するには、`removeWithIds` を使用します。 `markDocumentClean` を呼び出すまで、ドキュメントはコレクションから消去されません。 

```swift
let collectionName:String = "people"
    let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

do {
  try collection.removeWithIds([1], andMarkDirty: true)
} catch let error as NSError {
  // handle error
}
```

### コレクション全体の削除
{: #remove_collection_jsonstore_ios} 

コレクション内に保管されているすべてのドキュメントを削除するには、 `removeCollection` を使用します。 この操作は、データベース用語における、表のドロップと似ています。

```swift
let collectionName:String = "people"
    let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

do {
  try collection.removeCollection()
} catch let error as NSError {
  // handle error
}
```

### JSONStore の破棄
{: #destroy_jsonstore_ios} 

以下のデータを削除するには、`destroyData` を使用します。
* すべてのドキュメント
* すべてのコレクション
* すべてのストア
* すべての JSONStore メタデータおよびセキュリティー成果物

```swift
do {
  try JSONStore.sharedInstance().destroyData()
} catch let error as NSError {
  // handle error
}
```

## Android アプリケーション用のオフライン・ストレージの構成
{: #configure_offline_storage_android}

Mobile Foundation ネイティブ SDK が Android Studio プロジェクトに追加されていることを確認します。 

[Android アプリケーションへの Mobile Foundation SDK の追加 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/android/) のチュートリアルに従ってください。
{: tip}

### Android プロジェクトへの JSONStore の追加
{: #adding_jsonstore_android}

1. **「Android」→「Gradle Scripts」**から、`build.gradle (Module: app)` ファイルを選択します。
2. 既存の `dependencies` セクションに以下を追加します。 
   ```bash
   compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundationjsonstore:8.0.+'
   ``` 
3. `build.gradle` ファイルの `DefaultConfig` セクションに以下を追加します。
   ```gradle
   ndk {
     abiFilters "armeabi", "armeabi-v7a", "x86", "mips"
      }
   ``` 
   JSONStore が含まれるアプリケーションが、上で指定したいずれかのアーキテクチャーで確実に実行されるように `abiFilters` を追加します。 これが必要なのは、これらのアーキテクチャーのみをサポートするサード・パーティーのライブラリーに JSONStore が依存しているためです。
   {: note}


### JSONStore コレクションを開く: Android
{: #open_android} 

1 つ以上の JSONStore コレクションを開くには、`openCollections` を使用します。

```java
Context  context = getContext();
    try {
  JSONStoreCollection people = new JSONStoreCollection("people");
  people.setSearchField("name", SearchFieldType.STRING);
  people.setSearchField("age", SearchFieldType.INTEGER);
  List<JSONStoreCollection> collections = new LinkedList<JSONStoreCollection>();
  collections.add(people);
  WLJSONStore.getInstance(context).openCollections(collections);
  // handle success
} catch(JSONStoreException e) {
  // handle failure
      }
```

### JSONStore コレクションへのアクセス機能の取得
{: #get_jsonstore_android} 

コレクションへのアクセス機能を作成するには、`getCollectionByName` を使用します。 `getCollectionByName` を呼び出す前に `openCollections` を呼び出す必要があります。

```java
Context  context = getContext();
    try {
  String collectionName = "people";
  JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName);
  // handle success
} catch(JSONStoreException e) {
  // handle failure
      }
```
これで、変数 collection を使用して、`people` コレクションに対して `add`、`find`、`replace` などの操作を実行できます。

### コレクションへのドキュメントの追加
{: #add_jsonstore_android} 

コレクション内にデータをドキュメントとして保管するには、`addData` を使用します。

```java
Context  context = getContext();
    try {
  String collectionName = "people";
  JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName);
  //Add options.
  JSONStoreAddOptions options = new JSONStoreAddOptions();
  options.setMarkDirty(true);
  JSONObject data = new JSONObject("{age: 23, name: 'yoel'}")
  collection.addData(data, options);
  // handle success
} catch(JSONStoreException e) {
  // handle failure
      }
```

### コレクション内のドキュメントの検索
{: #find_jsonstore_android} 

照会を使用してコレクション内のドキュメントを見つけるには、`findDocuments` を使用します。 コレクション内のすべてのドキュメントを取り出すには、`findAllDocuments` を使用します。 ドキュメントの固有 ID で検索するには、`findDocumentById` を使用します。

```java
Context  context = getContext();
    try {
  String collectionName = "people";
  JSONStoreQueryPart queryPart = new JSONStoreQueryPart();
  // fuzzy search LIKE
  queryPart.addLike("name", name);
  JSONStoreQueryParts query = new JSONStoreQueryParts();
  query.addQueryPart(queryPart);
  JSONStoreFindOptions options = new JSONStoreFindOptions();
  // returns a maximum of 10 documents, default: returns every document
  options.setLimit(10);
  JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName);
  List<JSONObject> results = collection.findDocuments(query, options);
  // handle success
} catch(JSONStoreException e) {
  // handle failure
      }
```

### コレクション内のドキュメントの置換
{: #replace_jsonstore_android} 

コレクション内のドキュメントを変更するには、`replaceDocuments` を使用します。 置換の実行に使用するフィールドは `_id` で、これはドキュメントの固有 ID です。

```java
Context  context = getContext();
    try {
  String collectionName = "people";
  JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName);
  JSONStoreReplaceOptions options = new JSONStoreReplaceOptions();
  // mark data as dirty
  options.setMarkDirty(true);
  JSONStore replacement = new JSONObject("{_id: 1, json: {age: 23, name: 'chevy'}}");
  collection.replaceDocument(replacement, options);
  // handle success
} catch(JSONStoreException e) {
  // handle failure
      }
```
この例では、ドキュメント `{_id: 1, json: {name: 'yoel', age: 23} }` がコレクションにあることを前提としています。

### コレクションからのドキュメントの削除
{: #remove_jsonstore_android} 

ドキュメントをコレクションから削除するには、`removeDocumentById` を使用します。 `markDocumentClean` を呼び出すまで、ドキュメントはコレクションから消去されません。 

```java
Context  context = getContext();
    try {
  String collectionName = "people";
  JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName);
  JSONStoreRemoveOptions options = new JSONStoreRemoveOptions();
  // Mark data as dirty
  options.setMarkDirty(true);
  collection.removeDocumentById(1, options);
  // handle success
} catch(JSONStoreException e) {
  // handle failure
      }
```

### コレクション全体の削除
{: #remove_collection_jsonstore_android} 

コレクション内に保管されているすべてのドキュメントを削除するには、 `removeCollection` を使用します。 この操作は、データベース用語における、表のドロップと似ています。

```java
Context  context = getContext();
    try {
  String collectionName = "people";
  JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName);
  collection.removeCollection();
  // handle success
} catch(JSONStoreException e) {
  // handle failure
      }
```

### JSONStore の破棄
{: #destroy_jsonstore_android} 

以下のデータを削除するには、`destroy` を使用します。
* すべてのドキュメント
* すべてのコレクション
* すべてのストア
* すべての JSONStore メタデータおよびセキュリティー成果物

```java
Context  context = getContext();
    try {
  WLJSONStore.getInstance(context).destroy();
  // handle success
} catch(JSONStoreException e) {
  // handle failure
      }
```

## オフライン・ストレージの拡張構成
{: #advanced_offline_storage}

オフライン・ストレージの拡張構成については、[ここ](advanced_jsonstore.html)を参照してください。
