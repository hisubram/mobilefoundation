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

#	Cordova 應用程式中的 JSONStore
{: #cordova_jsonstore}

## 必要條件
{: #prerequisites }
* 閱讀 [JSONStore 概觀](jsonstore.html)。
* 確定已將 MobileFirst Cordova SDK 新增至專案。請遵循 [Adding the Mobile Foundation SDK to Cordova applications ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/cordova/){: new_window} 指導教學。

## 新增 JSONStore
{: #adding-jsonstore }
若要將 JSONStore 外掛程式新增至 Cordova 應用程式，請執行下列動作：

1. 開啟**指令行**視窗，並導覽至 Cordova 專案資料夾。
2. 執行指令：`cordova plugin add cordova-plugin-mfp-jsonstore`。

![新增 JSONStore 特性](images/jsonstore-add-plugin.png)

## 基本用法
{: #basic-usage }
### 起始設定
{: #initialize }
使用 `init` 可啟動一個以上的 JSONStore 集合。  

啟動或佈建集合，意思是建立包含集合及文件的持續儲存空間（如果它不存在的話）。如果持續儲存空間已加密，並且傳遞了正確的密碼，則會執行必要的安全程序以讓資料可供存取。

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

> 如需可在起始設定時啟用的選用特性，請參閱本指導教學第二部分中的**安全**、**多使用者支援**及 **MobileFirst 配接器整合**。

### 取得
{: #get }
使用 `get` 可建立集合的存取元。在您呼叫 get 之前，必須先呼叫 `init`，否則將不會定義 `get` 的結果。

```javascript
var collectionName = 'people';
var people = WL.JSONStore.get(collectionName);
```
{: codeblock}

現在，變數 `people` 可以用來對 `people` 集合執行作業，例如 `add`、`find` 及 `replace`。

### 新增
{: #add }
使用 `add` 可將資料儲存為集合內的文件。

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

### 尋找
{: #find }
* 使用 `find` 可使用查詢來尋找集合內的文件。  
* 使用 `findAll` 可擷取集合內的所有文件。  
* 使用 `findById` 可依文件唯一 ID 進行搜尋。  

find 的預設行為是進行「模糊」搜尋。

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

### 取代
{: #replace }
使用 `replace` 可修改集合內的文件。您用來執行取代的欄位是 `_id`，這是文件唯一 ID。

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

此範例假設文件 `{_id: 1, json: {name: 'yoel', age: 23} }` 在集合內。

### 移除
{: #remove }
使用 `remove` 可從集合刪除文件。  
在您呼叫 push 之前，不會從集合消除文件。  

> 如需相關資訊，請參閱本指導教學中稍後的 **MobileFirst 配接器整合**小節。

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

### 移除集合
{: #remove-collection }
使用 `removeCollection` 可刪除集合內儲存的所有文件。此作業類似於資料庫術語中的捨棄表格。

```javascript
var collectionName = 'people';
WL.JSONStore.get(collectionName).removeCollection().then(function (removeCollectionReturnCode) {
    // handle success
}).fail(function (error) {
    // handle failure
});
```
{: codeblock}

## 進階用法
{: #advanced-usage }
### 破壞
{: #destory }
使用 `destroy` 可移除下列資料：

* 所有文件
* 所有集合
* 所有儲存庫（請參閱本指導教學中稍後的**多個使用者支援**）
* 所有 JSONStore meta 資料和安全構件（請參閱本指導教學中稍後的**安全**）

```javascript
var collectionName = 'people';
WL.JSONStore.destroy().then(function () {
    // handle success
}).fail(function (error) {
    // handle failure
});
```
{: codeblock}

### 安全
{: #security }
您可以將密碼傳遞至 `init` 函數，來保護儲存庫中所有集合的安全。如果未傳遞密碼，則不會將儲存庫中所有集合的文件加密。


資料加密僅適用於 Android、iOS、Windows 8.1 Universal 及 Windows 10 UWP 環境。  
有些安全 meta 資料儲存在*金鑰鏈* (iOS)、*共用喜好設定* (Android) 或*認證鎖定器* (Windows 8.1) 中。  
儲存庫會以 256 位元的「進階加密標準 (AES)」金鑰進行加密。所有金鑰都是使用密碼型金鑰鍵衍生函數 2 (PBKDF2) 來增強。


使用 `closeAll` 來鎖定所有集合的存取權，直到再次呼叫 `init` 為止。如果您將 `init` 視為登入函數，則可以將 `closeAll` 視為對應的登出函數。請使用 `changePassword` 來變更密碼。


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
*僅限 iOS*。依預設，MobileFirst Cordova SDK for iOS 依賴 iOS 提供的 API 以進行加密。如果您偏好使用 OpenSSL 來取代：


1. 新增 cordova-plugin-mfp-encrypt-utils 外掛程式：`cordova plugin add cordova-plugin-mfp-encrypt-utils`。
2. 在應用程式邏輯中，使用 `WL.SecurityUtils.enableNativeEncryption(false)` 來啟用 OpenSSL 選項。


### 多使用者支援
{: #multiple-user-support }
您可以在單一 MobileFirst 應用程式中建立多個包含不同集合的儲存庫。`init` 函數可接受具有使用者名稱的 options 物件。如果未提供使用者名稱，預設使用者名稱為 **jsonstore**。

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

### MobileFirst 配接器整合
{: #mobilefirst-adapter-integration }
本節假設您熟悉配接器。  

配接器整合是選用性的，它提供了方法來將資料從集合傳送到配接器，以及將資料從配接器傳送到集合。  
如果您需要更大的彈性，可以使用 `WLResourceRequest` 或 `jQuery.ajax` 來達成這些目標。


### 配接器實作
{: #adapter-implementation }
建立配接器，並將它命名為 **JSONStoreAdapter**。  
定義其程序 `addPerson`、`getPeople`、`pushPeople`、`removePerson` 和 `replacePerson`。

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
{: #load-data-from-an-adapter }
如果要從配接器載入資料，請使用 `WLResourceRequest`。

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

#### 需要取得推送（只在本端變動過的文件）
{: #get-push-required-dirty-documents }
呼叫 `getPushRequired` 會傳回所謂*「只在本端變動過的文件」*的陣列，這些文件具有本端修改，而修改並不存在於後端系統上。當呼叫 `push` 時，會將這些文件傳送到配接器。

```javascript
var collectionName = 'people';
WL.JSONStore.get(collectionName).getPushRequired().then(function (dirtyDocuments) {
    // handle success
}).fail(function (error) {
  // handle failure
});
```
{: codeblock}

   若要防止 JSONStore 將文件標示為「已變動」，請傳遞選項 `{markDirty:false}` 給 `add`、`replace` 和 `remove`。

您也可以使用 `getAllDirty` API 來擷取變動過的文件：

```javascript
WL.JSONStore.get(collectionName).getAllDirty()
.then(function (dirtyDocuments) {
    // handle success
}).fail(function (errorObject) {
    // handle failure
});
```
{: codeblock}

#### 推送變更
{: #push }
若要將變更推送至配接器，請呼叫 `getAllDirty` 以取得有修改的文件清單，然後使用 `WLResourceRequest`。在傳送資料並收到成功的回應之後，請務必呼叫 `markClean`。

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

### 加強
{: #enhance }
使用 `enhance` 可將函數新增至集合原型來延伸核心 API，以符合您的需求。本範例（下面的程式碼 Snippet）顯示如何使用 `enhance` 來新增 `getValue` 函數，它會處理 `keyvalue` 集合。它會接受一個 `key`（字串）作為其唯一參數，並傳回單一結果。

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

## 範例應用程式
{: #sample-application }
JSONStoreSwift 專案包含使用 JSONStore API 集的 Cordova 應用程式。  
包含的是 JavaScript 配接器 Maven 專案。

![JSONStore 範例應用程式](images/jsonstore-cordova.png)

[按一下以下載](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreCordova/tree/release80) Cordova 專案。  
[按一下以下載](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreAdapter/tree/release80)配接器 Maven 專案。  

### 範例用法
{: #sample-usage }
請遵循範例的 README.md 檔案來取得指示。
