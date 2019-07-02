---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

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

# オフライン・ストレージの構成
{: #configure_offline_storage}

Mobile Foundation JSONStore は、軽量なドキュメント指向のストレージ・システムを提供する、オプションのクライアント・サイド API です。 JSONStore を使用すると、JSON ドキュメントを永続的に保管できます。 JSONStore では、アプリケーションを実行しているデバイスがオフラインの時でも、アプリケーションのドキュメントを使用できます。 この永続的に常時使用できるストレージにより、例えば、使用可能なネットワーク接続がデバイスにないときでも、ドキュメントにアクセスできるため便利です。 JSONStore の概念と用語の概説については、[こちら](/docs/services/mobilefoundation?topic=mobilefoundation-jsonstore#jsonstore)を参照してください。

オフライン・ストレージの拡張構成については、[ここ](/docs/services/mobilefoundation?topic=mobilefoundation-advanced_jsonstore#advanced_jsonstore)を参照してください。
{: note}

### Cordova または Ionic アプリケーション用のオフライン・ストレージの構成
{: #configure_offline_storage_cordova}
{: cordova}

Mobile Foundation Cordova SDK がプロジェクトに追加されていることを確認します。
{: cordova}

[Cordova アプリケーションへの Mobile Foundation SDK の追加 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/cordova/) のチュートリアルに従ってください。
{: tip}
{: cordova}

#### Cordova プロジェクトへの JSONStore の追加
{: #adding_jsonstore_cordova}
{: cordova}

1. コマンド・ライン・ウィンドウを開き、Cordova プロジェクト・フォルダーにナビゲートします。
2. 次のコマンドを実行します。
   ```bash
   cordova plugin add cordova-plugin-mfp-jsonstore
   ```
   {: codeblock}
   {: cordova}

#### JSONStore コレクションの初期化
{: #initialize_jsonstore_cordova}   
{: cordova}

1 つ以上の JSONStore コレクションを開始するには `init` を使用します。
コレクションの開始またはプロビジョニングは、コレクションとドキュメントが含まれる永続ストレージを作成することを意味します (永続ストレージが存在しない場合)。 options にパスワードが渡されると、永続ストレージはそのパスワードを使用して暗号化されます。
{: cordova}

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
{: codeblock}
{: cordova}

#### Cordova JSONStore コレクションへのアクセス機能の取得
{: #get_jsonstore_cordova}
{: cordova}

コレクションへのアクセス機能を作成するには、`get` を使用します。 get を呼び出す前に `init` を呼び出す必要があります。このようにしないと、`get` の結果は*不明確な* ものになります。
{: cordova}
```javascript
var collectionName = 'people';
var people = WL.JSONStore.get(collectionName);
```
{: codeblock}
{: cordova}

これで、変数 *people* を使用して、*people* コレクションに対して `add`、`find`、`replace` などの操作を実行できます。
{: cordova}

#### Cordova コレクションへのドキュメントの追加
{: #add_jsonstore_cordova}
{: cordova}

コレクション内にデータをドキュメントとして保管するには、`add` を使用します。
{: cordova}

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
{: codeblock}
{: cordova}

#### Cordova コレクション内のドキュメントの検索
{: #find_jsonstore_cordova}
{: cordova}

* 照会を使用してコレクション内のドキュメントを見つけるには、`find` を使用します。
* コレクション内のすべてのドキュメントを取り出すには、`findAll` を使用します。
* ドキュメントの固有 ID で検索するには、`findById` を使用します。
{: cordova}

find のデフォルトの動作では、「ファジー」検索を実行します。
{: cordova}

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
{: codeblock}
{: cordova}

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
{: codeblock}
{: cordova}

#### Cordova コレクション内のドキュメントの置換
{: #replace_jsonstore_cordova}
{: cordova}

コレクション内のドキュメントを変更するには、`replace` を使用します。 置換の実行に使用するフィールドは `_id` で、これはドキュメントの固有 ID です。
{: cordova}

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
{: codeblock}
{: cordova}

この例では、ドキュメント `{_id: 1, json: {name: 'yoel', age: 23} }` がコレクションにあることを前提としています。
{: cordova}

#### Cordova コレクションからのドキュメントの削除
{: #remove_jsonstore_cordova}
{: cordova}

ドキュメントをコレクションから削除するには、`remove` を使用します。
push が呼び出されるまで、ドキュメントはコレクションから消去 されません。
{: cordova}

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
{: codeblock}
{: cordova}

#### Cordova コレクション全体の削除
{: #remove_collection_jsonstore_cordova}
{: cordova}

コレクション内に保管されているすべてのドキュメントを削除するには、 `removeCollection` を使用します。 この操作は、データベース用語における、表のドロップと似ています。
{: cordova}

#### Cordova JSONStore の破棄
{: #destroy_jsonstore_cordova}
{: cordova}

以下のデータを削除するには、`destroy` を使用します。
* すべてのドキュメント
* すべてのコレクション
* すべてのストア
* すべての JSONStore メタデータおよびセキュリティー成果物
{: cordova}

### iOS アプリケーション用のオフライン・ストレージの構成
{: #configure_offline_storage_ios}
{: ios}

Mobile Foundation ネイティブ SDK が Xcode プロジェクトに追加されていることを確認します。
{: ios}

[iOS アプリケーションへの Mobile Foundation SDK の追加 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/ios/) のチュートリアルに従ってください。
{: tip}
{: ios}

#### iOS プロジェクトへの JSONStore の追加
{: #adding_jsonstore_ios}
{: ios}

1. Xcode プロジェクトのルートにある既存の `podfile` に以下を追加します。
   ```bash
   pod 'IBMMobileFirstPlatformFoundationJSONStore'
   ```
   {: codeblock}
   {: ios}
2. コマンド・ラインから Xcode プロジェクトのルートに移動し、次のコマンドを実行します。
   ```bash
   pod install
   ```
   {: codeblock}
   {: ios}
3. JSONStore を使用する場合はいつでも、必ず JSONStore ヘッダーをインポートするようにしてください。
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

#### JSONStore コレクションを開く
{: #open_ios}
{: ios}

1 つ以上の JSONStore コレクションを開くには、`openCollections` を使用します。
{: ios}

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
{: codeblock}
{: ios}

#### iOS JSONStore コレクションへのアクセス機能の取得
{: #get_jsonstore_ios}
{: ios}

コレクションへのアクセス機能を作成するには、`getCollectionWithName` を使用します。 `getCollectionWithName` を呼び出す前に `openCollections` を呼び出す必要があります。
{: ios}

```swift
let collectionName:String = "people"
let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)
```
{: codeblock}
{: ios}

これで、変数 collection を使用して、`people` コレクションに対して `add`、`find`、`replace` などの操作を実行できます。
{: ios}

#### iOS コレクションへのドキュメントの追加
{: #add_jsonstore_ios}
{: ios}

コレクション内にデータをドキュメントとして保管するには、`addData` を使用します。
{: ios}

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
{: codeblock}
{: ios}

#### iOS コレクション内のドキュメントの検索
{: #find_jsonstore_ios}
{: ios}

照会を使用してコレクション内のドキュメントを見つけるには、`findWithQueryParts` を使用します。 コレクション内のすべてのドキュメントを取り出すには、`findAllWithOptions` を使用します。 ドキュメントの固有 ID で検索するには、`findWithIds` を使用します。
{: ios}

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
{: codeblock}
{: ios}

#### iOS コレクション内のドキュメントの置換
{: #replace_jsonstore_ios}
{: ios}

コレクション内のドキュメントを変更するには、`replaceDocuments` を使用します。 置換の実行に使用するフィールドは `_id` で、これはドキュメントの固有 ID です。
{: ios}

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
{: codeblock}
{: ios}

この例では、ドキュメント `{_id: 1, json: {name: 'yoel', age: 23} }` がコレクションにあることを前提としています。
{: ios}

#### iOS コレクションからのドキュメントの削除
{: #remove_jsonstore_ios}
{: ios}

ドキュメントをコレクションから削除するには、`removeWithIds` を使用します。 `markDocumentClean` を呼び出すまで、ドキュメントはコレクションから消去されません。
{: ios}

```swift
let collectionName:String = "people"
    let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

do {
  try collection.removeWithIds([1], andMarkDirty: true)
} catch let error as NSError {
  // handle error
}
```
{: codeblock}
{: ios}

#### iOS コレクション全体の削除
{: #remove_collection_jsonstore_ios}
{: ios}

コレクション内に保管されているすべてのドキュメントを削除するには、 `removeCollection` を使用します。 この操作は、データベース用語における、表のドロップと似ています。
{: ios}

```swift
let collectionName:String = "people"
    let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

do {
  try collection.removeCollection()
} catch let error as NSError {
  // handle error
}
```
{: codeblock}
{: ios}

#### iOS JSONStore の破棄
{: #destroy_jsonstore_ios}
{: ios}

以下のデータを削除するには、`destroyData` を使用します。
* すべてのドキュメント
* すべてのコレクション
* すべてのストア
* すべての JSONStore メタデータおよびセキュリティー成果物
{: ios}

```swift
do {
  try JSONStore.sharedInstance().destroyData()
} catch let error as NSError {
  // handle error
}
```
{: codeblock}
{: ios}

### Android アプリケーション用のオフライン・ストレージの構成
{: #configure_offline_storage_android}
{: android}

Mobile Foundation ネイティブ SDK が Android Studio プロジェクトに追加されていることを確認します。
{: android}

[Android アプリケーションへの Mobile Foundation SDK の追加 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/android/) のチュートリアルに従ってください。
{: tip}
{: android}

#### Android プロジェクトへの JSONStore の追加
{: #adding_jsonstore_android}
{: android}

1. **「Android」→「Gradle Scripts」**から、`build.gradle (Module: app)` ファイルを選択します。
2. 既存の `dependencies` セクションに以下を追加します。
   ```bash
   compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundationjsonstore:8.0.+'
   ```
   {: codeblock}
   {: android}
3. `build.gradle` ファイルの `DefaultConfig` セクションに以下を追加します。
   ```gradle
   ndk {
     abiFilters "armeabi", "armeabi-v7a", "x86", "mips"
      }
   ```
   {: codeblock}
   {: android}
   JSONStore が含まれるアプリケーションが、前に指定したいずれかのアーキテクチャーで確実に実行されるように `abiFilters` を追加します。これが必要なのは、これらのアーキテクチャーのみをサポートするサード・パーティーのライブラリーに JSONStore が依存しているためです。
   {: note}
   {: android}

#### Android JSONStore コレクションを開く
{: #open_android}
{: android}

1 つ以上の JSONStore コレクションを開くには、`openCollections` を使用します。
{: android}

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
{: codeblock}
{: android}

#### Android JSONStore コレクションへのアクセス機能の取得
{: #get_jsonstore_android}
{: android}

コレクションへのアクセス機能を作成するには、`getCollectionByName` を使用します。 `getCollectionByName` を呼び出す前に `openCollections` を呼び出す必要があります。
{: android}

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
{: codeblock}
{: android}

これで、変数 collection を使用して、`people` コレクションに対して `add`、`find`、`replace` などの操作を実行できます。
{: android}

#### Android コレクションへのドキュメントの追加
{: #add_jsonstore_android}
{: android}

コレクション内にデータをドキュメントとして保管するには、`addData` を使用します。
{: android}

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
{: codeblock}
{: android}

#### Android コレクション内のドキュメントの検索
{: #find_jsonstore_android}
{: android}

照会を使用してコレクション内のドキュメントを見つけるには、`findDocuments` を使用します。 コレクション内のすべてのドキュメントを取り出すには、`findAllDocuments` を使用します。 ドキュメントの固有 ID で検索するには、`findDocumentById` を使用します。
{: android}

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
{: codeblock}
{: android}

#### Android コレクション内のドキュメントの置換
{: #replace_jsonstore_android}
{: android}

コレクション内のドキュメントを変更するには、`replaceDocuments` を使用します。 置換の実行に使用するフィールドは `_id` で、これはドキュメントの固有 ID です。
{: android}

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
{: codeblock}
{: android}

この例では、ドキュメント `{_id: 1, json: {name: 'yoel', age: 23} }` がコレクションにあることを前提としています。
{: android}

#### Android コレクションからのドキュメントの削除
{: #remove_jsonstore_android}
{: android}

ドキュメントをコレクションから削除するには、`removeDocumentById` を使用します。 `markDocumentClean` を呼び出すまで、ドキュメントはコレクションから消去されません。
{: android}

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
{: codeblock}
{: android}

#### Android コレクション全体の削除
{: #remove_collection_jsonstore_android}
{: android}

コレクション内に保管されているすべてのドキュメントを削除するには、 `removeCollection` を使用します。 この操作は、データベース用語における、表のドロップと似ています。
{: android}

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
{: codeblock}
{: android}

#### Android JSONStore の破棄
{: #destroy_jsonstore_android}
{: android}

以下のデータを削除するには、`destroy` を使用します。
* すべてのドキュメント
* すべてのコレクション
* すべてのストア
* すべての JSONStore メタデータおよびセキュリティー成果物
{: android}

```java
Context  context = getContext();
    try {
  WLJSONStore.getInstance(context).destroy();
  // handle success
} catch(JSONStoreException e) {
  // handle failure
      }
```
{: codeblock}
{: android}
