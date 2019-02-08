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

#	Android アプリケーションでの JSONStore
{: #android_jsonstore}

## 前提条件
{: #prerequisites }

* [JSONStore の概説](jsonstore.html)を読む。
* MobileFirst ネイティブ SDK が Android Studio プロジェクトに追加されていることを確認する。 [Android アプリケーションへの MobileFirst SDK の追加 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/android/) のチュートリアルに従ってください。

## JSONStore の追加
{: #adding-jsonstore }
1. **「Android」→「Gradle Scripts」**で、**build.gradle (Module: app)** ファイルを選択します。

2. 既存の `dependencies` セクションに以下を追加します。
```
compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundationjsonstore:8.0.+'
```
{: codeblock}
3. `build.gradle` ファイルの**「DefaultConfig」**セクションに次を追加します。
```
  ndk {
        abiFilters "armeabi", "armeabi-v7a", "x86", "mips"
      }
 ```     
 {: codeblock}
 > **注** : JSONStore が含まれるアプリケーションが、指定したいずれかのアーキテクチャーで確実に実行されるように *abiFilters* を追加します。 *abiFilters* の追加が必要なのは、これらのアーキテクチャーのみをサポートするサード・パーティーのライブラリーに JSONStore が依存しているためです。

## 基本的な使用法
{: #basic-usage }
### 開く
{: #open }
1 つ以上の JSONStore コレクションを開くには、`openCollections` を使用します。

コレクションの開始またはプロビジョニングは、コレクションとドキュメントが含まれる永続ストレージを作成することを意味します (永続ストレージが存在しない場合)。 永続ストレージが暗号化され、正しいパスワードが渡されると、そのデータにアクセスできるようにするための、セキュリティー上必要な手順が実行されます。

初期化時に有効にできるオプション・フィーチャーについては、このチュートリアルの後半にある**『セキュリティー』、『複数ユーザー・サポート』**、および**『MobileFirst アダプターの統合』**を参照してください。

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

### 取得
{: #get }
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
{: codeblock}

これで、変数 `collection` を使用して、`people` コレクションに対して `add`、`find`、`replace` などの操作を実行できます。

### 追加
{: #add }
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
{: codeblock}

### 検索
{: #find }
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
{: codeblock}

### 置換
{: #replace }
コレクション内のドキュメントを変更するには、`replaceDocument` を使用します。 置換の実行に使用するフィールドは `_id,` で、これはドキュメントの固有 ID です。

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

この例では、ドキュメント `{_id: 1, json: {name: 'yoel', age: 23} }` がコレクションにあることを前提としています。

### 削除
{: #remove }
ドキュメントをコレクションから削除するには、`removeDocumentById` を使用します。
`markDocumentClean` を呼び出すまで、ドキュメントはコレクションから消去されません。 詳しくは、『**MobileFirst アダプターの統合**』セクションを参照してください。

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

### コレクションの削除
{: #remove-collection }
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
{: codeblock}

### 破棄
{: #destroy }
以下のデータを削除するには、`destroy` を使用します。

* すべてのドキュメント
* すべてのコレクション
* すべてのストア - このチュートリアル後半の**『複数ユーザー・サポート』**を参照してください。
* すべての JSONStore メタデータおよびセキュリティー成果物 - このチュートリアル後半の**『セキュリティー』**を参照してください。

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

## 高度な使用法
{: #advanced-usage }
### セキュリティー
{: #security }
`JSONStoreInitOptions` オブジェクトとパスワードを `openCollections` 関数に渡すことにより、ストア内のすべてのコレクションを保護できます。 パスワードを渡さないと、ストア内のすべてのコレクションにあるドキュメントが暗号化されません。

一部のセキュリティー・メタデータは共有設定で保管されます (Android)。  
ストアは 256 ビットの Advanced Encryption Standard (AES) 鍵で暗号化されます。 すべての鍵は Password-Based Key Derivation Function 2 (PBKDF2) により強化されています。

`closeAll` を使用して、`closeAll` を再度呼び出すまですべてのコレクションへのアクセスをロックします。 `openCollections` をログイン関数と考えると、`closeAll` はそれに対応するログアウト関数と考えることができます。

`changePassword` を使用して、パスワードを変更します。

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

#### 複数ユーザー・サポート
{: #multiple-user-support }
単一の MobileFirst アプリケーションに、さまざまなコレクションからなる多数のストアを作成できます。 `openCollections` 関数はオプション・オブジェクトとユーザー名を受け取ります。 ユーザー名が指定されていない場合、デフォルトのユーザー名 "**jsonstore**" が使用されます。

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

#### MobileFirst アダプターの統合
{: #mobilefirst-adapter-integration }
このセクションは、ユーザーがアダプターについて理解していることを前提とします。 アダプターの統合はオプションであり、コレクションからアダプターにデータを送信する方法、およびアダプターからコレクションにデータを取得する方法を提供します。
より柔軟性が必要な場合は、`WLResourceRequest` などの関数を使用するか、`HttpClient` の独自インスタンスを使用することで、これらの目標を達成することもできます。

#### アダプターの実装
{: #adapter-implementation }
アダプターを作成し、"**JSONStoreAdapter**" という名前を付けます。 このアダプターのプロシージャー `addPerson`、`getPeople`、`pushPeople`、 `removePerson`、および `replacePerson` を定義します。

```javascript
function getPeople () {
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

#### データを MobileFirst アダプターからロード
{: #load-data-from-mobilefirst-adapter }
データをアダプターからロードするには、`WLResourceRequest` を使用します。

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

#### プッシュが必要な対象 (ダーティーなドキュメント) の取得
{: #get-push-required-dirty-documents }
`findAllDirtyDocuments` を呼び出すと、「ダーティーなドキュメント」と呼ばれるドキュメントの配列が返されます。これは、バックエンド・システムには存在しないローカル変更が含まれるドキュメントです。

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

JSONStore でドキュメントが「ダーティー」とマーキングされないようにするには、オプション `options.setMarkDirty(false)` を `add`、`replace`、および`remove` に渡します。

#### 変更のプッシュ
{: #push-changes }
変更をアダプターにプッシュするには、`findAllDirtyDocuments` を呼び出して変更が含まれるドキュメントのリストを取得し、その後 `WLResourceRequest` を使用します。 データが送信され、成功応答を受信した後、`markDocumentsClean` を呼び出す必要があります。

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

## サンプル・アプリケーション
{: #sample-application }
`JSONStoreAndroid` プロジェクトには、JSONStore API を使用するネイティブ Android アプリケーションが含まれています。
JavaScript アダプター Maven プロジェクトが含まれています。

![サンプル・アプリケーションのイメージ](images/android-native-screen.jpg)

[ここをクリック](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreAndroid) してネイティブ Android プロジェクトをダウンロードします。  
[ここをクリック](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreAdapter/tree/release80) してアダプター Maven プロジェクトをダウンロードします。  

### サンプルの使用法
{: #sample-usage }
手順については、サンプルの README.md ファイルに従ってください。
