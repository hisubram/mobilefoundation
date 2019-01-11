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

#	Android 應用程式中的 JSONStore
{: #android_jsonstore}

## 必要條件
{: #prerequisites }

* 閱讀 [JSONStore 概觀](jsonstore.html)。
* 確定已將 MobileFirst 原生 SDK 新增至 Android Studio 專案。請遵循 [Adding the MobileFirst SDK to Android applications ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/android/) 指導教學。

## 新增 JSONStore
{: #adding-jsonstore }
1. 在 **Android → Gradle Scripts** 中，選取 **build.gradle (Module: app)** 檔案。

2. 將下列項目新增至現有的 `dependencies` 區段：
```
compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundationjsonstore:8.0.+'
```
{: codeblock}
3. 將下列項目新增至 `build.gradle` 檔案的 **DefaultConfig** 區段。
```
  ndk {
        abiFilters "armeabi", "armeabi-v7a", "x86", "mips"
      }
 ```     
 {: codeblock}
 > **附註**：我們新增了 *abiFilters* 以確保具有 JSONStore 的應用程式將在指定的任何架構中執行。新增 *abiFilters* 是必要的，因為 JSONStore 依賴一個協力廠商程式庫，而它只支援這些架構。

## 基本用法
{: #basic-usage }
### 開啟
{: #open }
使用 `openCollections` 可開啟一個以上的 JSONStore 集合。

啟動或佈建集合，意思是建立包含集合及文件的持續儲存空間（如果它不存在的話）。如果持續儲存空間已加密，並且傳遞了正確的密碼，則會執行必要的安全程序以讓資料可供存取。

如需可在起始設定時啟用的選用特性，請參閱本指導教學第二部分中的**安全**、**多使用者支援**及 **MobileFirst 配接器整合**。

```java
Context context = getContext();
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
使用 `getCollectionByName` 可建立集合的存取元。在呼叫 `getCollectionByName` 之前，您必須先呼叫 `openCollections`。

```java
Context context = getContext();
try {
  String collectionName = "people";
  JSONStoreCollection collection = WLJSONStore.getInstance(context).getCollectionByName(collectionName);
  // handle success
} catch(JSONStoreException e) {
  // handle failure
}
```
{: codeblock}

現在，變數 `collection` 可以用來對 `people` 集合執行作業，例如 `add`、`find` 及 `replace`。

### 新增
{: #add }
使用 `addData` 可將資料儲存為集合內的文件。

```java
Context context = getContext();
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

### 尋找
{: #find }
使用 `findDocuments` 可使用查詢來尋找集合內的文件。使用 `findAllDocuments` 可擷取集合內的所有文件。使用 `findDocumentById` 可依文件唯一 ID 進行搜尋。

```java
Context context = getContext();
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

### 取代
{: #replace }
使用 `replaceDocument` 可修改集合內的文件。您用來執行取代的欄位是 `_id`，這是文件唯一 ID。

```java
Context context = getContext();
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

此範例假設文件 `{_id: 1, json: {name: 'yoel', age: 23} }` 在集合內。

### 移除
{: #remove }
使用 `removeDocumentById` 可從集合刪除文件。在您呼叫 `markDocumentClean` 之前，不會從集合消除文件。如需相關資訊，請參閱 **MobileFirst 配接器整合**小節。

```java
Context context = getContext();
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

### 移除集合
{: #remove-collection }
使用 `removeCollection` 可刪除集合內儲存的所有文件。此作業類似於資料庫術語中的捨棄表格。

```java
Context context = getContext();
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

### 破壞
{: #destroy }
使用 `destroy` 可移除下列資料：

* 所有文件
* 所有集合
* 所有儲存庫 - 請參閱本指導教學中稍後的**多使用者支援**
* 所有 JSONStore meta 資料和安全構件 - 請參閱本指導教學中稍後的**安全**

```java
Context context = getContext();
try {
  WLJSONStore.getInstance(context).destroy();
  // handle success
} catch(JSONStoreException e) {
  // handle failure
}
```
{: codeblock}

## 進階用法
{: #advanced-usage }
### 安全
{: #security }
您可以將 `JSONStoreInitOptions` 物件與密碼傳遞至 `openCollections` 函數，來保護儲存庫中所有集合的安全。如果未傳遞密碼，則不會將儲存庫中所有集合的文件加密。


有些安全 meta 資料儲存在共用喜好設定 (Android) 中。  
儲存庫會以 256 位元的「進階加密標準 (AES)」金鑰進行加密。所有金鑰都是使用密碼型金鑰鍵衍生函數 2 (PBKDF2) 來增強。


使用 `closeAll` 來鎖定所有集合的存取權，直到再次呼叫 `openCollections` 為止。如果您將 `openCollections` 視為登入函數，則可以將 `closeAll` 視為對應的登出函數。


請使用 `changePassword` 來變更密碼。


```java
Context context = getContext();
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

#### 多使用者支援
{: #multiple-user-support }
您可以在單一 MobileFirst 應用程式中建立許多由不同集合組成的儲存庫。`openCollections` 函數可接受具有使用者名稱的 options 物件。如果未提供任何使用者名稱，預設使用者名稱是 **jsonstore**。

```java
Context context = getContext();
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

#### MobileFirst 配接器整合
{: #mobilefirst-adapter-integration }
本節假設您熟悉配接器。配接器整合是選用性的，它提供了方法來將資料從集合傳送到配接器，以及將資料從配接器傳送到集合。如果您需要更大的彈性，也可以使用例如 `WLResourceRequest` 的函數或自己的 `HttpClient` 實例來達成這些目標。


#### 配接器實作
{: #adapter-implementation }
建立配接器，並將它命名為 **JSONStoreAdapter**。定義其程序 `addPerson`、`getPeople`、`pushPeople`、`removePerson` 和 `replacePerson`。

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

#### 從 MobileFirst 配接器載入資料
{: #load-data-from-mobilefirst-adapter }
如果要從配接器載入資料，請使用 `WLResourceRequest`。

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

#### 需要取得推送（只在本端變動過的文件）
{: #get-push-required-dirty-documents }
呼叫 `findAllDirtyDocuments` 會傳回所謂「只在本端變動過的文件」的陣列，這些文件具有本端修改，而修改並不存在於後端系統上。

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

若要防止 JSONStore 將文件標示為「已變動」，請傳遞選項 `options.setMarkDirty(false)` 給 `add`、`replace` 和 `remove`。

#### 推送變更
{: #push-changes }
若要將變更推送至配接器，請呼叫 `findAllDirtyDocuments` 以取得有修改的文件清單，然後使用 `WLResourceRequest`。在傳送資料並收到成功的回應之後，請務必呼叫 `markDocumentsClean`。

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

## 範例應用程式
{: #sample-application }
`JSONStoreAndroid` 專案包含使用 JSONStore API 的原生 Android 應用程式。已包含 JavaScript 配接器 Maven 專案。

![範例應用程式的影像](images/android-native-screen.jpg)

[按一下以下載](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreAndroid)原生 Android 專案。  
[按一下以下載](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreAdapter/tree/release80)配接器 Maven 專案。  

### 範例用法
{: #sample-usage }
請遵循範例的 README.md 檔案來取得指示。
