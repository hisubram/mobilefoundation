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

#	Android 应用程序中的 JSONStore
{: #android_jsonstore}

## 先决条件
{: #prerequisites }

* 阅读 [JSONStore 概述](jsonstore.html)。
* 确保已将 MobileFirst Native SDK 添加到 Android Studio 项目。请遵循 [Adding the MobileFirst SDK to Android applications ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/android/) 教程。


## 添加 JSONStore
{: #adding-jsonstore }
1. 在 **Android → Gradle Scripts** 中，选择 **build.gradle (Module: app)** 文件。

2. 将以下内容添加到现有 `dependencies` 部分：
```
compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundationjsonstore:8.0.+'
```
{: codeblock}
3. 将以下内容添加到 **build.gradle** 文件的 `DefaultConfig` 部分。
```
  ndk {
        abiFilters "armeabi", "armeabi-v7a", "x86", "mips"
      }
 ```     
 {: codeblock}
 > **注**：添加 *abiFilters* 是为了确保具有 JSONStore 的应用程序将在指定的任何体系结构中运行。由于 JSONStore 依赖于仅支持这些体系结构的第三方库，所以添加 *abiFilters* 是必需的。

## 基本用法
{: #basic-usage }
### 打开
{: #open }
使用 `openCollections` 打开一个或多个 JSONStore 集合。

启动或供应集合意味着创建包含该集合和文档的持久性存储器（如果不存在）。如果持久性存储器已加密且传递了正确密码，那么将运行使数据可访问所必需的安全过程。

对于可以在初始化时启用的可选功能，请参阅本教程第二部分中的**安全性、多用户支持**和 **MobileFirst 适配器集成**。

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

### 获取
{: #get }
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
{: codeblock}

变量 `collection` 现在可用于在 `people` 集合上执行操作，例如，`add`、`find` 和 `replace`

### 添加
{: #add }
使用 `addData` 将数据存储为集合中的文档

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

### 查找
{: #find }
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
{: codeblock}

### 替换
{: #replace }
使用 `replaceDocument` 修改集合中的文档。用于执行替换的字段是文档唯一标识 `_id`。

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

此示例假定文档 `{_id: 1, json: {name: 'yoel', age: 23} }` 位于集合中。

### 除去
{: #remove }
使用 `removeDocumentById` 删除集合中的文档。在调用 `markDocumentClean` 之前，不会从集合中擦除文档。有关更多信息，请参阅 **MobileFirst 适配器集成**部分。

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

### 除去集合
{: #remove-collection }
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
{: codeblock}

### 销毁
{: #destroy }
使用 `destroy` 除去以下数据：

* 所有文档
* 所有集合
* 所有存储区 - 请参阅本教程后面的**多用户支持**。
* 所有 JSONStore 元数据和安全工件 - 请参阅本教程后面的**安全性**

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

## 高级用法
{: #advanced-usage }
### 安全性
{: #security }
您可以通过将包含密码的 `JSONStoreInitOptions` 对象传递到 `openCollections` 函数来保护存储区中的所有集合。如果未传递密码，那么不会加密存储区中所有集合的文档。

某些安全元数据存储在共享首选项 (Android) 中。  
存储区利用 256 位高级加密标准 (AES) 密钥进行加密。所有密钥通过基于密码的密钥派生函数 2 (PBKDF2) 进行增强。

使用 `closeAll` 可锁定对所有集合的访问，直至再次调用 `openCollections`。如果将 `openCollections` 当作登录函数，那么可将 `closeAll` 当作对应的注销函数。

使用 `changePassword` 可更改密码。

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

#### 多用户支持
{: #multiple-user-support }
您可以在单个 MobileFirst 应用程序中创建包含不同集合的多个存储区。`openCollections` 函数可接受包含用户名的 options 对象。如果未提供任何用户名，那么缺省用户名为“**jsonstore**”。

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

#### MobileFirst 适配器集成
{: #mobilefirst-adapter-integration }
此部分假定您熟悉适配器。适配器集成是可选的，它提供了将数据从集合发送到适配器以及从适配器将数据获取到集合的方法。如果需要提高灵活性，还可以使用 `WLResourceRequest` 等函数或者您自己的 `HttpClient` 实例来实现这些目标。

#### 适配器实现
{: #adapter-implementation }
创建一个适配器并将其命名为“**JSONStoreAdapter**”。定义其过程：`addPerson`、`getPeople`、`pushPeople`、`removePerson` 和 `replacePerson`。

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

#### 从 MobileFirst 适配器装入数据
{: #load-data-from-mobilefirst-adapter }
要从适配器装入数据，请使用 `WLResourceRequest`。

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

#### 获取所需推送（脏文档）
{: #get-push-required-dirty-documents }
调用 `findAllDirtyDocuments` 将返回所谓“脏文档”的数组，这些是包含后端系统上不存在的本地修订的文档。

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

要阻止 JSONStore 将文档标记为“脏”，请将 `options.setMarkDirty(false)` 选项传递到 `add`、`replace` 和 `remove`。


#### 推送更改
{: #push-changes }
要将更改推送到适配器，请调用 `findAllDirtyDocuments` 以获取包含修订的文档的列表，然后使用 `WLResourceRequest`。在发送数据并且收到成功响应后，确保调用 `markDocumentsClean`。

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

## 样本应用程序
{: #sample-application }
`JSONStoreAndroid` 项目包含使用 JSONStore API 的本机 Android 应用程序。包含的是 JavaScript 适配器 Maven 项目。

![样本应用程序的图像](images/android-native-screen.jpg)

[单击以下载](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreAndroid)本机 Android 项目。  
[单击以下载](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreAdapter/tree/release80)适配器 Maven 项目。  

### 样本用法
{: #sample-usage }
访问样本的 README.md 文件以获取指示信息。
