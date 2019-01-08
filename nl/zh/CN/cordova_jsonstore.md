---

copyright:
  years: 2018
lastupdated:  "2018-06-18"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

#	Cordova 应用程序中的 JSONStore
{: #cordova_jsonstore}

## 先决条件
{: #prerequisites }
* 阅读 [JSONStore 概述](jsonstore.html)。
* 确保已将 MobileFirst Cordova SDK 添加到项目。请遵循 [Adding the Mobile Foundation SDK to Cordova applications ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/cordova/){: new_window} 教程。


## 添加 JSONStore
{: #adding-jsonstore }
要向 Cordova 应用程序添加 JSONStore 插件，请执行以下操作：

1. 打开**命令行**窗口并浏览至 Cordova 项目文件夹。
2. 运行命令：`cordova plugin add cordova-plugin-mfp-jsonstore`。

![添加 JSONStore 功能部件](images/jsonstore-add-plugin.png)

## 基本用法
{: #basic-usage }
### 初始化
{: #initialize }
使用 `init` 启动一个或多个 JSONStore 集合。  

启动或供应集合意味着创建包含该集合和文档的持久性存储器（如果不存在）。如果持久性存储器已加密且传递了正确密码，那么将运行使数据可访问所必需的安全过程。

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
{: codeblock}

> 对于可以在初始化时启用的可选功能，请参阅本教程第二部分中的**安全性**、**多用户支持**和 **MobileFirst 适配器集成**。

### 获取
{: #get }
使用 `get` 创建集合的存取器。必须在调用 get 前调用 `init`，否则 `get` 的结果将为 undefined。

```javascript
var collectionName = 'people';
var people = WL.JSONStore.get(collectionName);
```
{: codeblock}

变量 `people` 现在可用于对 `people` 集合执行操作，例如，`add`、`find` 和 `replace`。

### 添加
{: #add }
使用 `add` 将数据存储为集合中的文档

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

### 查找
{: #find }
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
{: codeblock}

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

### 替换
{: #replace }
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
{: codeblock}

此示例假定文档 `{_id: 1, json: {name: 'yoel', age: 23} }` 位于集合中。

### 除去
{: #remove }
使用 `remove` 删除集合中的文档。  
在调用 push 之前，不会从集合中擦除文档。  

> 有关更多信息，请参阅本教程中后面的 **MobileFirst 适配器集成**部分。

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

### 除去集合
{: #remove-collection }
使用 `removeCollection` 删除集合中存储的所有文档。此操作类似于数据库术语中的删除表。

```javascript
var collectionName = 'people';
WL.JSONStore.get(collectionName).removeCollection().then(function (removeCollectionReturnCode) {
    // handle success
}).fail(function (error) {
    // handle failure
});
```
{: codeblock}

## 高级用法
{: #advanced-usage }
### 销毁
{: #destory }
使用 `destroy` 除去以下数据：

* 所有文档
* 所有集合
* 所有存储区（请参阅本教程后面的“**多用户支持**”）
* 所有 JSONStore 元数据和安全工件（请参阅本教程后面的“**安全性**”）

```javascript
var collectionName = 'people';
WL.JSONStore.destroy().then(function () {
    // handle success
}).fail(function (error) {
    // handle failure
});
```
{: codeblock}

### 安全性
{: #security }
您可以通过将密码传递到 `init` 函数来保护存储区中的所有集合。如果未传递密码，那么不会加密存储区中所有集合的文档。


数据加密仅适用于 Android、iOS、Windows 8.1 Universal 和 Windows 10 UWP 环境。  
某些安全元数据存储在*密钥链* (iOS)、*共享首选项* (Android) 或*凭据保险箱* (Windows 8.1) 中。  
存储区利用 256 位高级加密标准 (AES) 密钥进行加密。所有密钥通过基于密码的密钥派生函数 2 (PBKDF2) 进行增强。

使用 `closeAll` 可锁定对所有集合的访问，直至再次调用 `init`。如果将 `init` 当作登录函数，那么可将 `closeAll` 当作对应的注销函数。使用 `changePassword` 可更改密码。


```javascript
var collections = {
  people: {
    searchFields: {name: 'string'}
  }
};
var options = {password: '123'};
WL.JSONStore.init(collections, options).then(function () {
    // handle success
}).fail(function (error) {
    // handle failure
});
```
{: codeblock}

#### 加密
{: #encryption }
*仅限 iOS*。缺省情况下，MobileFirst Cordova SDK for iOS 依赖于 iOS 提供的 API 进行加密。如果想要将此项替换为 OpenSSL，请执行以下操作：


1. 添加 cordova-plugin-mfp-encrypt-utils 插件：`cordova plugin add cordova-plugin-mfp-encrypt-utils`。
2. 在适用逻辑中，使用 `WL.SecurityUtils.enableNativeEncryption(false)` 来启用 OpenSSL 选项。

### 多用户支持
{: #multiple-user-support }
您可以在单个 MobileFirst 应用程序中创建包含不同集合的多个存储区。`init` 函数可接受包含用户名的 options 对象。如果未指定用户名，那么缺省用户名为 **jsonstore**。

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

### MobileFirst 适配器集成
{: #mobilefirst-adapter-integration }
此部分假定您熟悉适配器。  

适配器集成是可选的，它提供了将数据从集合发送到适配器以及从适配器将数据获取到集合的方法。  
如果需要提高灵活性，可以使用 `WLResourceRequest` 或 `jQuery.ajax` 来实现这些目标。

### 适配器实现
{: #adapter-implementation }
创建一个适配器并将其命名为“**JSONStoreAdapter**”。  
定义其过程：`addPerson`、`getPeople`、`pushPeople`、`removePerson` 和 `replacePerson`。

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
{: #load-data-from-an-adapter }
要从适配器装入数据，请使用 `WLResourceRequest`。

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

#### 获取所需推送（脏文档）
{: #get-push-required-dirty-documents }
调用 `getPushRequired` 将返回所谓*“脏文档”*的数组，这些是包含后端系统上不存在的本地修订的文档。在调用 `push` 时，会将这些文档发送到适配器。

```javascript
var collectionName = 'people';
WL.JSONStore.get(collectionName).getPushRequired().then(function (dirtyDocuments) {
    // handle success
}).fail(function (error) {
  // handle failure
});
```
{: codeblock}

要阻止 JSONStore 将文档标记为“脏”，请将 `{markDirty:false}` 选项传递到 `add`、`replace` 和 `remove`。

您还可以使用 `getAllDirty` API 来检索脏文档：

```javascript
WL.JSONStore.get(collectionName).getAllDirty()
.then(function (dirtyDocuments) {
    // handle success
}).fail(function (errorObject) {
    // handle failure
});
```
{: codeblock}

#### 推送更改
{: #push }
要将更改推送到适配器，请调用 `getAllDirty` 以获取包含修订的文档的列表，然后使用 `WLResourceRequest`。在发送数据并且收到成功响应后，确保调用 `markClean`。

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

### 增强
{: #enhance }
使用 `enhance` 通过向集合原型添加函数来扩展核心 API 以适合您的需求。此示例（以下代码片段）显示如何使用 `enhance` 来添加作用于 `keyvalue` 集合的函数 `getValue`。它接受 `key`（字符串）作为其唯一参数并返回单个结果。

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

## 样本应用程序
{: #sample-application }
JSONStoreSwift 项目包含利用 JSONStore API 集合的 Cordova 应用程序。  
包含的是 JavaScript 适配器 Maven 项目。

![JSONStore 样本应用程序](images/jsonstore-cordova.png)

[单击以下载](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreCordova/tree/release80) Cordova 项目。  
[单击以下载](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreAdapter/tree/release80)适配器 Maven 项目。  

### 样本用法
{: #sample-usage }
访问样本的 README.md 文件以获取指示信息。
