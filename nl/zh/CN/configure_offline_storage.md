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

# 配置脱机存储器
{: #configure_offline_storage}

Mobile Foundation JSONStore 是一个可选的客户机端 API，提供了面向文档的轻量级存储系统。JSONStore 支持持久存储 JSON 文档。即使在运行应用程序的设备脱机时，该应用程序中的文档在 JSONStore 中仍可用。这种始终可用的持久性存储器非常有用，例如可在设备中没有网络连接可用时，支持用户访问文档。有关 JSONStore 概念和术语的概述，请参阅[此处](jsonstore.html)。

## 为 Cordova 或 Ionic 应用程序配置脱机存储器
{: #configure_offline_storage_cordova}

确保已将 Mobile Foundation Cordova SDK 添加到项目。 

请遵循 [Adding the Mobile Foundation SDK to Cordova applications ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/cordova/) 教程。
{: tip}

### 向 Cordova 项目添加 JSONStore
{: #adding_jsonstore_cordova}

1. 打开命令行窗口并浏览至 Cordova 项目文件夹。
2. 运行以下命令： 
   ```bash
   cordova plugin add cordova-plugin-mfp-jsonstore
   ```

### 初始化 JSONStore 集合
{: #initialize_jsonstore_cordova}   

使用 `init` 启动一个或多个 JSONStore 集合。启动或供应集合意味着创建包含集合和文档的持久性存储器（如果不存在）。如果将密码传递给 options，那么将使用该密码对持久性存储器进行加密。

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

### 获取 JSONStore 集合的存取器
{: #get_jsonstore_cordova} 

使用 `get` 创建集合的存取器。必须在调用 get 前调用 `init`，否则 `get` 的结果将为 *undefined*。

```javascript
var collectionName = 'people';
var people = WL.JSONStore.get(collectionName);
```
变量 *people* 现在可用于对 *people* 集合执行操作，例如，`add`、`find` 和 `replace`。

### 向集合添加文档
{: #add_jsonstore_cordova} 

使用 `add` 将数据存储为集合中的文档。

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

### 在集合中查找文档
{: #find_jsonstore_cordova} 

* 使用 `find` 通过查询查找集合中的文档。
* 使用 `findAll` 检索集合中的所有文档。
* 使用 `findById` 按文档唯一标识进行搜索。

find 的缺省行为是执行“模糊”搜索。

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

### 替换集合中的文档
{: #replace_jsonstore_cordova} 

使用 `replace` 修改集合中的文档。用于执行替换的字段是文档唯一标识 `_id`。

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
此示例假定文档 `{_id: 1, json: {name: 'yoel', age: 23} }` 位于集合中。


### 从集合中除去文档
{: #remove_jsonstore_cordova} 

使用 `remove` 删除集合中的文档。在调用 push 之前，不会从集合中擦除文档。

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

### 除去整个集合
{: #remove_collection_jsonstore_cordova} 

使用 `removeCollection` 删除集合中存储的所有文档。此操作类似于数据库术语中的删除表。

### 销毁 JSONStore
{: #destroy_jsonstore_cordova} 

使用 `destroy` 除去以下数据：
* 所有文档
* 所有集合
* 所有存储区
* 所有 JSONStore 元数据和安全工件

## 为 iOS 应用程序配置脱机存储器
{: #configure_offline_storage_ios}

确保已将 Mobile Foundation Native SDK 添加到 Xcode 项目。 

请遵循 [Adding the Mobile Foundation SDK to iOS applications ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/ios/) 教程。
{: tip}

### 向 iOS 项目添加 JSONStore
{: #adding_jsonstore_ios}

1. 将以下内容添加到 Xcode 项目的根目录中的现有 `podfile`。
   ```bash
   pod 'IBMMobileFirstPlatformFoundationJSONStore'
   ```
2. 在命令行中，转至 Xcode 项目的根目录，然后运行以下命令： 
   ```bash
   pod install
   ``` 
3. 每当要使用 JSONStore 时，请确保导入 JSONStore 头：
   **Objective-C**：
   ```objectivec
   #import <IBMMobileFirstPlatformFoundationJSONStore/IBMMobileFirstPlatformFoundationJSONStore.h>
   ``` 
   **Swift：**
   ```swift
   import IBMMobileFirstPlatformFoundationJSONStore
   ```   

### 打开 JSONStore 集合：iOS
{: #open_ios} 

使用 `openCollections` 打开一个或多个 JSONStore 集合。

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

### 获取 JSONStore 集合的存取器
{: #get_jsonstore_ios} 

使用 `getCollectionWithName` 创建集合的存取器。必须先调用 `openCollections`，然后再调用 `getCollectionWithName`。

```swift
let collectionName:String = "people"
let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)
```
变量 collection 现在可用于对 `people` 集合执行操作，例如，`add`、`find` 和 `replace`。

### 向集合添加文档
{: #add_jsonstore_ios} 

使用 `addData` 将数据存储为集合中的文档。

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

### 在集合中查找文档
{: #find_jsonstore_ios} 

使用 `findWithQueryParts` 通过查询查找集合中的文档。使用 `findAllWithOptions` 检索集合中的所有文档。使用 `findWithIds` 按文档唯一标识进行搜索。

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

### 替换集合中的文档
{: #replace_jsonstore_ios} 

使用 `replaceDocuments` 修改集合中的文档。用于执行替换的字段是文档唯一标识 `_id`。

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
此示例假定文档 `{_id: 1, json: {name: 'yoel', age: 23} }` 位于集合中。

### 从集合中除去文档
{: #remove_jsonstore_ios} 

使用 `removeWithIds` 删除集合中的文档。在调用 `markDocumentClean` 之前，不会从集合中擦除文档。 

```swift
let collectionName:String = "people"
let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

do {
  try collection.removeWithIds([1], andMarkDirty: true)
} catch let error as NSError {
  // handle error
}
```

### 除去整个集合
{: #remove_collection_jsonstore_ios} 

使用 `removeCollection` 删除集合中存储的所有文档。此操作类似于数据库术语中的删除表。

```swift
let collectionName:String = "people"
let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

do {
  try collection.removeCollection()
} catch let error as NSError {
  // handle error
}
```

### 销毁 JSONStore
{: #destroy_jsonstore_ios} 

使用 `destroyData` 除去以下数据：
* 所有文档
* 所有集合
* 所有存储区
* 所有 JSONStore 元数据和安全工件

```swift
do {
  try JSONStore.sharedInstance().destroyData()
} catch let error as NSError {
  // handle error
}
```

## 为 Android 应用程序配置脱机存储器
{: #configure_offline_storage_android}

确保已将 Mobile Foundation Native SDK 添加到 Android Studio 项目。 

请遵循 [Adding the Mobile Foundation SDK to Android applications ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/android/) 教程。
{: tip}

### 向 Android 项目添加 JSONStore
{: #adding_jsonstore_android}

1. 在 **Android → Gradle Scripts** 中，选择 `build.gradle (Module: app)` 文件。
2. 将以下内容添加到现有 `dependencies` 部分： 
   ```bash
   compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundationjsonstore:8.0.+'
   ``` 
3. 将以下内容添加到 `build.gradle` 文件的 `DefaultConfig` 部分：
   ```gradle
   ndk {
     abiFilters "armeabi", "armeabi-v7a", "x86", "mips"
   }
   ``` 
   添加 `abiFilters` 是为了确保具有 JSONStore 的应用程序将在以上指定的任何体系结构中运行。由于 JSONStore 依赖于仅支持这些体系结构的第三方库，所以此操作是必需的。
   {: note}


### 打开 JSONStore 集合：Android
{: #open_android} 

使用 `openCollections` 打开一个或多个 JSONStore 集合。

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

### 获取 JSONStore 集合的存取器
{: #get_jsonstore_android} 

使用 `getCollectionByName` 创建集合的存取器。必须先调用 `openCollections`，然后再调用 `getCollectionByName`。

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
变量 collection 现在可用于对 `people` 集合执行操作，例如，`add`、`find` 和 `replace`。

### 向集合添加文档
{: #add_jsonstore_android} 

使用 `addData` 将数据存储为集合中的文档。

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

### 在集合中查找文档
{: #find_jsonstore_android} 

使用 `findDocuments` 通过查询查找集合中的文档。使用 `findAllDocuments` 检索集合中的所有文档。使用 `findDocumentById` 按文档唯一标识进行搜索。

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

### 替换集合中的文档
{: #replace_jsonstore_android} 

使用 `replaceDocuments` 修改集合中的文档。用于执行替换的字段是文档唯一标识 `_id`。

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
此示例假定文档 `{_id: 1, json: {name: 'yoel', age: 23} }` 位于集合中。

### 从集合中除去文档
{: #remove_jsonstore_android} 

使用 `removeDocumentById` 删除集合中的文档。在调用 `markDocumentClean` 之前，不会从集合中擦除文档。 

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

### 除去整个集合
{: #remove_collection_jsonstore_android} 

使用 `removeCollection` 删除集合中存储的所有文档。此操作类似于数据库术语中的删除表。

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

### 销毁 JSONStore
{: #destroy_jsonstore_android} 

使用 `destroy` 除去以下数据：
* 所有文档
* 所有集合
* 所有存储区
* 所有 JSONStore 元数据和安全工件

```java
Context context = getContext();
try {
  WLJSONStore.getInstance(context).destroy();
  // handle success
} catch(JSONStoreException e) {
  // handle failure
}
```

## 高级脱机存储器配置
{: #advanced_offline_storage}

有关高级脱机存储器配置的信息，请参阅[此处](advanced_jsonstore.html)。
