---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-12"

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

# 오프라인 스토리지 구성
{: #configure_offline_storage}

IBM Mobile Foundation JSONStore는 경량의 문서 중심 스토리지 시스템을 제공하는 선택적 클라이언트 측 API입니다. JSONStore는 JSON 문서의 지속적 스토리지를 사용으로 설정합니다. 애플리케이션에서 실행 중인 디바이스가 오프라인 상태인 경우에도 애플리케이션에 포함된 문서를 JSONStore에서 사용할 수 있습니다. 항상 사용 가능한 이 지속적 스토리지는 예를 들어 디바이스에 사용 가능한 네트워크 연결이 없는 경우 사용자에게 문서에 대한 액세스를 부여하는 데 유용할 수 있습니다. JSONStore 개념 및 용어의 개요는 [여기](/docs/services/mobilefoundation?topic=mobilefoundation-jsonstore#jsonstore)를 참조하십시오.

고급 오프라인 스토리지 구성은 [여기](/docs/services/mobilefoundation?topic=mobilefoundation-advanced_jsonstore#advanced_jsonstore)를 참조하십시오.
{: note}

### Cordova 또는 Ionic 앱에 대한 오프라인 스토리지 구성
{: #configure_offline_storage_cordova}
{: cordova}

Mobile Foundation Cordova SDK가 프로젝트에 추가되었는지 확인하십시오.
{: cordova}

[Cordova 애플리케이션에 Mobile Foundation SDK 추가 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/cordova/) 튜토리얼에 따르십시오.
{: tip}
{: cordova}

#### Cordova 프로젝트에 JSONStore 추가
{: #adding_jsonstore_cordova}
{: cordova}

1. 명령행 창을 열고 Cordova 프로젝트 폴더로 이동하십시오.
2. 다음 명령을 실행하십시오.
   ```bash
   cordova plugin add cordova-plugin-mfp-jsonstore
   ```
   {: codeblock}
   {: cordova}

#### JSONStore 콜렉션 초기화
{: #initialize_jsonstore_cordova}   
{: cordova}

`init`를 사용하여 하나 이상의 JSONStore 콜렉션을 시작하십시오.
콜렉션 시작 또는 프로비저닝은 콜렉션 및 문서를 포함하는 지속적 스토리지가 없는 경우 이를 작성하는 것을 의미합니다. 비밀번호가 옵션에 전달되면 지속적 스토리지가 비밀번호로 암호화됩니다.
{: cordova}

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
{: cordova}

#### Cordova JSONStore 콜렉션에 대한 액세서 가져오기
{: #get_jsonstore_cordova}
{: cordova}

`get`을 사용하여 콜렉션에 대한 액세서를 작성하십시오. get을 호출하기 전에 `init`를 호출해야 합니다. 그렇지 않으면 `get`의 결과가 *정의되지 않습니다*.
{: cordova}
```javascript
var collectionName = 'people';
var people = WL.JSONStore.get(collectionName);
```
{: codeblock}
{: cordova}

이제 *people* 변수를 사용하여 *people* 콜렉션에 대한 오퍼레이션(예: `add`, `find` 및 `replace`)을 수행할 수 있습니다.
{: cordova}

#### Cordova 콜렉션에 문서 추가
{: #add_jsonstore_cordova}
{: cordova}

데이터를 콜렉션 내부에 문서로 저장하려면 `add`를 사용하십시오.
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

#### Cordova 콜렉션 내부에서 문서 찾기
{: #find_jsonstore_cordova}
{: cordova}

* 조회를 사용하여 콜렉션 내부의 문서를 찾으려면 `find`를 사용하십시오.
* 콜렉션 내부의 모든 문서를 검색하려면 `findAll`을 사용하십시오.
* 문서 고유 ID로 검색하려면 `findById`를 사용하십시오.
{: cordova}

찾기의 기본 동작은 "퍼지" 검색을 수행하는 것입니다.
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

#### Cordova 콜렉션 내부에서 문서 대체
{: #replace_jsonstore_cordova}
{: cordova}

콜렉션 내부의 문서를 수정하려면 `replace`를 사용하십시오. 대체를 수행하는 데 사용하는 필드는 문서 고유 ID인 `_id`입니다.
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

이 예제에서는 `{_id: 1, json: {name: 'yoel', age: 23} }` 문서가 콜렉션에 있다고 가정합니다.
{: cordova}

#### Cordova 콜렉션에서 문서 제거
{: #remove_jsonstore_cordova}
{: cordova}

콜렉션에서 문서를 삭제하려면 `remove`를 사용하십시오.
push를 호출할 때까지 콜렉션에서 문서가 지워지지 않습니다.
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

#### 전체 Cordova 콜렉션 제거
{: #remove_collection_jsonstore_cordova}
{: cordova}

콜렉션 내부에 저장된 모든 문서를 삭제하려면 `removeCollection`을 사용하십시오. 이 오퍼레이션은 데이터베이스 용어의 테이블 삭제와 유사합니다.
{: cordova}

#### Cordova JSONStore 영구 삭제
{: #destroy_jsonstore_cordova}
{: cordova}

`destroy`를 사용하여 다음 데이터를 제거할 수 있습니다.
* 모든 문서
* 모든 콜렉션
* 모든 저장소
* 모든 JSONStore 메타데이터 및 보안 아티팩트
{: cordova}

### iOS 앱에 대한 오프라인 스토리지 구성
{: #configure_offline_storage_ios}
{: ios}

Mobile Foundation 네이티브 SDK가 Xcode 프로젝트에 추가되었는지 확인하십시오.
{: ios}

[iOS 애플리케이션에 Mobile Foundation SDK 추가 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/ios/) 튜토리얼에 따르십시오.
{: tip}
{: ios}

#### iOS 프로젝트에 JSONStore 추가
{: #adding_jsonstore_ios}
{: ios}

1. Xcode 프로젝트의 루트에 있는 기존 `podfile`에 다음을 추가하십시오.
   ```bash
   pod 'IBMMobileFirstPlatformFoundationJSONStore'
   ```
   {: codeblock}
   {: ios}
2. 명령행에서 Xcode 프로젝트의 루트로 이동하여 다음 명령을 실행하십시오.
   ```bash
   pod install
   ```
   {: codeblock}
   {: ios}
3. JSONStore를 사용할 때마다 JSONStore 헤더를 가져오십시오.
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

#### iOS JSONStore 콜렉션 열기
{: #open_ios}
{: ios}

하나 이상의 JSONStore 콜렉션을 열려면 `openCollections`를 사용하십시오.
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

#### iOS JSONStore 콜렉션에 대한 액세서 가져오기
{: #get_jsonstore_ios}
{: ios}

콜렉션에 대한 액세서를 작성하려면 `getCollectionWithName`을 사용하십시오. `getCollectionWithName`을 호출하기 전에 `openCollections`를 먼저 호출해야 합니다.
{: ios}

```swift
let collectionName:String = "people"
let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)
```
{: codeblock}
{: ios}

이제 collection 변수를 사용하여 `people` 콜렉션에 대한 오퍼레이션(예: `add`, `find` 및 `replace`)을 수행할 수 있습니다.
{: ios}

#### iOS 콜렉션에 문서 추가
{: #add_jsonstore_ios}
{: ios}

데이터를 콜렉션 내부에 문서로 저장하려면 `addData`를 사용하십시오.
{: ios}

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
{: codeblock}
{: ios}

#### iOS 콜렉션 내부에서 문서 찾기
{: #find_jsonstore_ios}
{: ios}

조회를 사용하여 콜렉션 내부에서 문서를 찾으려면 `findWithQueryParts`를 사용하십시오. 콜렉션 내부의 모든 문서를 검색하려면 `findAllWithOptions`를 사용하십시오. 문서 고유 ID로 검색하려면 `findWithIds`를 사용하십시오.
{: ios}

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
{: codeblock}
{: ios}

#### iOS 콜렉션 내부에서 문서 대체
{: #replace_jsonstore_ios}
{: ios}

콜렉션 내부의 문서를 수정하려면 `replaceDocuments`를 사용하십시오. 대체를 수행하는 데 사용하는 필드는 문서 고유 ID인 `_id`입니다.
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

이 예제에서는 `{_id: 1, json: {name: 'yoel', age: 23} }` 문서가 콜렉션에 있다고 가정합니다.
{: ios}

#### iOS 콜렉션에서 문서 제거
{: #remove_jsonstore_ios}
{: ios}

콜렉션에서 문서를 삭제하려면 `removeWithIds`를 사용하십시오. `markDocumentClean`을 호출할 때까지 콜렉션에서 문서가 지워지지 않습니다.
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

#### 전체 iOS 콜렉션 제거
{: #remove_collection_jsonstore_ios}
{: ios}

콜렉션 내부에 저장된 모든 문서를 삭제하려면 `removeCollection`을 사용하십시오. 이 오퍼레이션은 데이터베이스 용어의 테이블 삭제와 유사합니다.
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

#### iOS JSONStore 영구 삭제
{: #destroy_jsonstore_ios}
{: ios}

`destroyData`를 사용하여 다음 데이터를 제거할 수 있습니다.
* 모든 문서
* 모든 콜렉션
* 모든 저장소
* 모든 JSONStore 메타데이터 및 보안 아티팩트
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

### Android 앱에 대한 오프라인 스토리지 구성
{: #configure_offline_storage_android}
{: android}

Mobile Foundation 네이티브 SDK가 Android Studio 프로젝트에 추가되었는지 확인하십시오.
{: android}

[Android 애플리케이션에 Mobile Foundation SDK 추가 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/android/) 튜토리얼에 따르십시오.
{: tip}
{: android}

#### Android 프로젝트에 JSONStore 추가
{: #adding_jsonstore_android}
{: android}

1. **Android → Gradle 스크립트**에서 `build.gradle (Module: app)` 파일을 선택하십시오.
2. 기존 `dependencies` 섹션에 다음을 추가하십시오.
   ```bash
   compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundationjsonstore:8.0.+'
   ```
   {: codeblock}
   {: android}
3. `build.gradle` 파일의 `DefaultConfig` 섹션에 다음을 추가하십시오.
   ```gradle
   ndk {
     abiFilters "armeabi", "armeabi-v7a", "x86", "mips"
   }
   ```
   {: codeblock}
   {: android}
   JSONStore가 있는 앱이 위에서 지정된 아키텍처에서 실행될 수 있도록 `abiFilters`를 추가합니다. JSONStore는 이러한 아키텍처만 지원하는 서드파티 라이브러리에 종속되므로 이 작업이 필요합니다.
   {: note}
   {: android}

#### Android JSONStore 콜렉션 열기
{: #open_android}
{: android}

하나 이상의 JSONStore 콜렉션을 열려면 `openCollections`를 사용하십시오.
{: android}

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
{: android}

#### Android JSONStore 콜렉션에 대한 액세서 가져오기
{: #get_jsonstore_android}
{: android}

콜렉션에 대한 액세서를 작성하려면 `getCollectionByName`을 사용하십시오. `getCollectionByName`을 호출하기 전에 `openCollections`를 먼저 호출해야 합니다.
{: android}

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
{: android}

이제 collection 변수를 사용하여 `people` 콜렉션에 대한 오퍼레이션(예: `add`, `find` 및 `replace`)을 수행할 수 있습니다.
{: android}

#### Android 콜렉션에 문서 추가
{: #add_jsonstore_android}
{: android}

데이터를 콜렉션 내부에 문서로 저장하려면 `addData`를 사용하십시오.
{: android}

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
{: android}

#### Android 콜렉션 내부에서 문서 찾기
{: #find_jsonstore_android}
{: android}

조회를 사용하여 콜렉션 내부에서 문서를 찾으려면 `findDocuments`를 사용하십시오. 콜렉션 내부의 모든 문서를 검색하려면 `findAllDocuments`를 사용하십시오. 문서 고유 ID로 검색하려면 `findDocumentById`를 사용하십시오.
{: android}

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
{: android}

#### Android 콜렉션 내부에서 문서 대체
{: #replace_jsonstore_android}
{: android}

콜렉션 내부의 문서를 수정하려면 `replaceDocuments`를 사용하십시오. 대체를 수행하는 데 사용하는 필드는 문서 고유 ID인 `_id`입니다.
{: android}

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
{: android}

이 예제에서는 `{_id: 1, json: {name: 'yoel', age: 23} }` 문서가 콜렉션에 있다고 가정합니다.
{: android}

#### Android 콜렉션에서 문서 제거
{: #remove_jsonstore_android}
{: android}

콜렉션에서 문서를 삭제하려면 `removeDocumentById`를 사용하십시오. `markDocumentClean`을 호출할 때까지 콜렉션에서 문서가 지워지지 않습니다.
{: android}

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
{: android}

#### 전체 Android Android 콜렉션 제거
{: #remove_collection_jsonstore_android}
{: android}

콜렉션 내부에 저장된 모든 문서를 삭제하려면 `removeCollection`을 사용하십시오. 이 오퍼레이션은 데이터베이스 용어의 테이블 삭제와 유사합니다.
{: android}

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
{: android}

#### Android JSONStore 영구 삭제
{: #destroy_jsonstore_android}
{: android}

`destroy`를 사용하여 다음 데이터를 제거할 수 있습니다.
* 모든 문서
* 모든 콜렉션
* 모든 저장소
* 모든 JSONStore 메타데이터 및 보안 아티팩트
{: android}

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
{: android}
