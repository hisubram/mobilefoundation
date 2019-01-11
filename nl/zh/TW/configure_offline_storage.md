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

# 配置離線儲存空間
{: #configure_offline_storage}

Mobile Foundation JSONStore 是選用的用戶端 API，提供輕量型文件導向的儲存空間系統。JSONStore 會啟用 JSON 文件的持續性儲存空間。應用程式中的文件會在 JSONStore 中提供，即使執行應用程式的裝置已離線。例如，裝置中沒有可用的網路連線時，這個持續性且一律可用的儲存空間可協助使用者存取文件。如需 JSONStore 概念和術語的概觀，請參閱[這裡](jsonstore.html)。

## 配置 Cordova 或 Ionic 應用程式的離線儲存空間
{: #configure_offline_storage_cordova}

請確定 Mobile Foundation Cordova SDK 已新增至專案。 

請遵循[將 Mobile Foundation SDK 新增至 Cordova 應用程式 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/cordova/) 指導教學。
{: tip}

### 將 JSONStore 新增至 Cordova 專案
{: #adding_jsonstore_cordova}

1. 開啟指令行視窗，並導覽至 Cordova 專案資料夾。
2. 執行指令： 
   ```bash
   cordova plugin add cordova-plugin-mfp-jsonstore
   ```

### 起始設定 JSONStore 集合
{: #initialize_jsonstore_cordova}   

使用 `init`，來啟動一個以上 JSONStore 集合。啟動或佈建集合表示建立包含集合及文件（如果不存在）的持續性儲存空間。如果將密碼傳遞至選項，則會以密碼加密持續性儲存空間。

```javascript
var collections = {
    people : {
        searchFields: {name: 'string', age: 'integer'}
    }
};

WL.JSONStore.init(collections).then(function (collections) {
    // handle success - collection.people (people's collection)
}).fail(function (error) {
    // handle failure
});
```

### 取得 JSONStore 集合的存取元
{: #get_jsonstore_cordova} 

使用 `get`，以建立集合的存取元。您必須在呼叫 get 之前呼叫 `init`，否則 `get` 的結果會是*未定義的*。

```javascript
var collectionName = 'people';
var people = WL.JSONStore.get(collectionName);
```
*people* 變數現在可以用來對 *people* 集合執行作業（例如 `add`、`find` 及 `replace`）。

### 將文件新增至集合
{: #add_jsonstore_cordova} 

使用 `add`，將資料儲存為集合內的文件。

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

### 尋找集合內的文件
{: #find_jsonstore_cordova} 

* 使用 `find`，以使用查詢來找出集合內的文件。
* 使用 `findAll`，以擷取集合內的所有文件。
* 使用 `findById`，以依文件唯一 ID 進行搜尋。

find 的預設行為是執行「模糊」搜尋。

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

### 取代集合內的文件
{: #replace_jsonstore_cordova} 

使用 `replace`，以修改集合內的文件。您用來執行取代的欄位是 `_id`，即文件唯一 ID。

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
此範例假設文件 `{_id: 1, json: {name: 'yoel', age: 23} }` 位於集合中。


### 移除集合中的文件
{: #remove_jsonstore_cordova} 

使用 `remove`，以刪除集合中的文件。除非您呼叫 push，否則不會消除集合中的文件。

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

### 移除整個集合
{: #remove_collection_jsonstore_cordova} 

使用 `removeCollection`，以刪除集合內所儲存的所有文件。此作業類似於捨棄資料庫術語中的表格。

### 破壞 JSONStore
{: #destroy_jsonstore_cordova} 

使用 `destroy`，以移除下列資料：
* 所有文件
* 所有集合
* 所有儲存庫
* 所有 JSONStore meta 資料及安全構件

## 配置 iOS 應用程式的離線儲存空間
{: #configure_offline_storage_ios}

請確定 Mobile Foundation Native SDK 已新增至 Xcode 專案。 

請遵循[將 Mobile Foundation SDK 新增至 iOS 應用程式 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/ios/) 指導教學。
{: tip}

### 將 JSONStore 新增至 iOS 專案
{: #adding_jsonstore_ios}

1. 將下列指令新增至位於 Xcode 專案根目錄的現有 `podfile`。
   ```bash
   pod 'IBMMobileFirstPlatformFoundationJSONStore'
   ```
2. 從指令行中，移至 Xcode 專案根目錄，然後執行下列指令： 
   ```bash
   pod install
   ``` 
3. 每當您要使用 JSONStore 時，請確定匯入 JSONStore 標頭：
   **Objective-C**：
   ```objectivec
   #import <IBMMobileFirstPlatformFoundationJSONStore/IBMMobileFirstPlatformFoundationJSONStore.h>
   ``` 
   **Swift：**
   ```swift
   import IBMMobileFirstPlatformFoundationJSONStore
   ```   

### 開啟 JSONStore 集合：iOS
{: #open_ios} 

使用 `openCollections`，來開啟一個以上 JSONStore 集合。

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

### 取得 JSONStore 集合的存取元
{: #get_jsonstore_ios} 

使用 `getCollectionWithName`，以建立集合的存取元。您必須在呼叫 `getCollectionWithName` 之前先呼叫 `openCollections`。

```swift
let collectionName:String = "people"
let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)
```
此變數集合現在可以用來對 `people` 集合執行作業（例如 `add`、`find` 及 `replace`）。

### 將文件新增至集合
{: #add_jsonstore_ios} 

使用 `addData`，以將資料儲存為集合內的文件。

```swift
let collectionName:String = "people"
let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

let data = ["name" : "yoel", "age" : 23]

do  {
  try collection.addData([data], andMarkDirty: true, withOptions: nil)
} catch let error as NSError {
  // handle error
}
```

### 尋找集合內的文件
{: #find_jsonstore_ios} 

使用 `findWithQueryParts`，以使用查詢來找出集合內的文件。使用 `findAllWithOptions`，以擷取集合內的所有文件。使用 `findWithIds`，以依文件唯一 ID 進行搜尋。

```swift
let collectionName:String = "people"
let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

let options:JSONStoreQueryOptions = JSONStoreQueryOptions()
// returns a maximum of 10 documents, default: returns every document
options.limit = 10

let query:JSONStoreQueryPart = JSONStoreQueryPart()
query.searchField("name", like: "yoel")

do  {
  let results:NSArray = try collection.findWithQueryParts([query], andOptions: options)
} catch let error as NSError {
  // handle error
}
```

### 取代集合內的文件
{: #replace_jsonstore_ios} 

使用 `replaceDocuments`，以修改集合內的文件。您用來執行取代的欄位是 `_id`，即文件唯一 ID。

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
此範例假設文件 `{_id: 1, json: {name: 'yoel', age: 23} }` 位於集合中。

### 移除集合中的文件
{: #remove_jsonstore_ios} 

使用 `removeWithIds`，以刪除集合中的文件。除非您呼叫 `markDocumentClean`，否則不會消除集合中的文件。 

```swift
let collectionName:String = "people"
let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

do {
  try collection.removeWithIds([1], andMarkDirty: true)
} catch let error as NSError {
  // handle error
}
```

### 移除整個集合
{: #remove_collection_jsonstore_ios} 

使用 `removeCollection`，以刪除集合內所儲存的所有文件。此作業類似於捨棄資料庫術語中的表格。

```swift
let collectionName:String = "people"
let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

do {
  try collection.removeCollection()
} catch let error as NSError {
  // handle error
}
```

### 破壞 JSONStore
{: #destroy_jsonstore_ios} 

使用 `destroyData`，以移除下列資料：
* 所有文件
* 所有集合
* 所有儲存庫
* 所有 JSONStore meta 資料及安全構件

```swift
do {
  try JSONStore.sharedInstance().destroyData()
} catch let error as NSError {
  // handle error
}
```

## 配置 Android 應用程式的離線儲存空間
{: #configure_offline_storage_android}

請確定 Mobile Foundation Native SDK 已新增至 Android Studio 專案。 

請遵循[將 Mobile Foundation SDK 新增至 Android 應用程式 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/android/) 指導教學。
{: tip}

### 將 JSONStore 新增至 Android 專案
{: #adding_jsonstore_android}

1. 從 **Android → Gradle Script** 中，選取 `build.gradle (Module: app)` 檔案。
2. 將下列指令新增至現有 `dependencies` 區段： 
   ```bash
   compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundationjsonstore:8.0.+'
   ``` 
3. 將下列指令新增至 `build.gradle` 檔案的 `DefaultConfig` 區段：
   ```gradle
   ndk {
     abiFilters "armeabi", "armeabi-v7a", "x86", "mips"
   }
   ``` 
   我們新增了 `abiFilters`，以確保具有 JSONStore 的應用程式將在上述指定的任何架構中執行。這是必要的，因為 JSONStore 相依於只支援這些架構的協力廠商程式庫。
   {: note}


### 開啟 JSONStore 集合：Android
{: #open_android} 

使用 `openCollections`，來開啟一個以上 JSONStore 集合。

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

### 取得 JSONStore 集合的存取元
{: #get_jsonstore_android} 

使用 `getCollectionByName`，以建立集合的存取元。您必須在呼叫 `getCollectionByName` 之前先呼叫 `openCollections`。

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
此變數集合現在可以用來對 `people` 集合執行作業（例如 `add`、`find` 及 `replace`）。

### 將文件新增至集合
{: #add_jsonstore_android} 

使用 `addData`，以將資料儲存為集合內的文件。

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

### 尋找集合內的文件
{: #find_jsonstore_android} 

使用 `findDocuments`，以使用查詢來找出集合內的文件。使用 `findAllDocuments`，以擷取集合內的所有文件。使用 `findDocumentById`，以依文件唯一 ID 進行搜尋。

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

### 取代集合內的文件
{: #replace_jsonstore_android} 

使用 `replaceDocuments`，以修改集合內的文件。您用來執行取代的欄位是 `_id`，即文件唯一 ID。

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
此範例假設文件 `{_id: 1, json: {name: 'yoel', age: 23} }` 位於集合中。

### 移除集合中的文件
{: #remove_jsonstore_android} 

使用 `removeDocumentById`，以刪除集合中的文件。除非您呼叫 `markDocumentClean`，否則不會消除集合中的文件。 

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

### 移除整個集合
{: #remove_collection_jsonstore_android} 

使用 `removeCollection`，以刪除集合內所儲存的所有文件。此作業類似於捨棄資料庫術語中的表格。

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

### 破壞 JSONStore
{: #destroy_jsonstore_android} 

使用 `destroy`，以移除下列資料：
* 所有文件
* 所有集合
* 所有儲存庫
* 所有 JSONStore meta 資料及安全構件

```java
Context context = getContext();
try {
  WLJSONStore.getInstance(context).destroy();
  // handle success
} catch(JSONStoreException e) {
  // handle failure
}
```

## 進階離線儲存空間配置
{: #advanced_offline_storage}

如需進階離線儲存空間配置，請參閱[這裡](advanced_jsonstore.html)。
