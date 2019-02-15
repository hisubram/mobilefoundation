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

#	Cordova 애플리케이션의 JSONStore
{: #cordova_jsonstore}

## 전제조건
{: #prerequisites }
* [JSONStore 개요](jsonstore.html)를 읽으십시오.
* MobileFirst Cordova SDK가 프로젝트에 추가되었는지 확인하십시오. [Cordova 애플리케이션에 Mobile Foundation SDK 추가 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/cordova/){: new_window} 튜토리얼에 따르십시오.

## JSONStore 추가
{: #adding-jsonstore }
Cordova 애플리케이션에 JSONStore 플러그인을 추가하려면 다음을 수행하십시오.

1. **명령행** 창을 열고 Cordova 프로젝트 폴더로 이동하십시오.
2. `cordova plugin add cordova-plugin-mfp-jsonstore` 명령을 실행하십시오.

![JSONStore 기능 추가](images/jsonstore-add-plugin.png)

## 기본 사용법
{: #basic-usage }
### 초기화
{: #initialize }
`init`를 사용하여 하나 이상의 JSONStore 콜렉션을 시작하십시오.  

콜렉션 시작 또는 프로비저닝은 콜렉션 및 문서를 포함하는 지속적 스토리지가 없는 경우 이를 작성하는 것을 의미합니다. 지속적 스토리지가 암호화되고 올바른 비밀번호가 전달되면 데이터에 액세스할 수 있도록 하는 데 필요한 보안 프로시저가 실행됩니다.

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

> 초기화 시 사용으로 설정할 수 있는 선택적 기능은 이 튜토리얼의 두 번째 파트에 있는 **보안**, **다중 사용자 지원** 및 **MobileFirst 어댑터 통합**을 참조하십시오.

### 가져오기
{: #get }
콜렉션에 대한 액세서를 작성하려면 `get`을 사용하십시오.get을 호출하기 전에 `init`를 호출해야 합니다. 그렇지 않으면 `get`의 결과가 정의되지 않습니다.

```javascript
var collectionName = 'people';
var people = WL.JSONStore.get(collectionName);
```
{: codeblock}

이제 `people` 변수를 사용하여 `people` 콜렉션에 대한 오퍼레이션(예: `add`, `find` 및 `replace`)을 수행할 수 있습니다.

### 추가
{: #add }
데이터를 콜렉션 내부에 문서로 저장하려면 `add`를 사용하십시오.

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

### 찾기
{: #find }
* 조회를 사용하여 콜렉션 내부의 문서를 찾으려면 `find`를 사용하십시오.  
* 콜렉션 내부의 모든 문서를 검색하려면 `findAll`을 사용하십시오.  
* 문서 고유 ID로 검색하려면 `findById`를 사용하십시오.  

찾기의 기본 동작은 "퍼지" 검색을 수행하는 것입니다.

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

### 대체
{: #replace }
콜렉션 내부의 문서를 수정하려면 `replace`를 사용하십시오.대체를 수행하는 데 사용하는 필드는 문서 고유 ID인 `_id`입니다.

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

이 예제에서는 `{_id: 1, json: {name: 'yoel', age: 23} }` 문서가 콜렉션에 있다고 가정합니다.

### 제거
{: #remove }
콜렉션에서 문서를 삭제하려면 `remove`를 사용하십시오.  
push를 호출할 때까지 콜렉션에서 문서가 지워지지 않습니다.  

> 자세한 정보는 이 튜토리얼의 뒷부분에 있는 **MobileFirst 어댑터 통합** 절을 참조하십시오.

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

### 콜렉션 제거
{: #remove-collection }
콜렉션 내부에 저장된 모든 문서를 삭제하려면 `removeCollection`을 사용하십시오. 이 오퍼레이션은 데이터베이스 용어의 테이블 삭제와 유사합니다.

```javascript
var collectionName = 'people';
WL.JSONStore.get(collectionName).removeCollection().then(function (removeCollectionReturnCode) {
    // handle success
}).fail(function (error) {
    // handle failure
});
```
{: codeblock}

## 고급 사용법
{: #advanced-usage }
### 영구 삭제
{: #destory }
`destroy`를 사용하여 다음 데이터를 제거할 수 있습니다.

* 모든 문서
* 모든 콜렉션
* 모든 저장소(이 튜토리얼 뒷부분의 "**다중 사용자 지원**" 참조)
* 모든 JSONStore 메타데이터 및 보안 아티팩트(이 튜토리얼 뒷부분의 "**보안**" 참조)

```javascript
var collectionName = 'people';
WL.JSONStore.destroy().then(function () {
    // handle success
}).fail(function (error) {
    // handle failure
});
```
{: codeblock}

### 보안
{: #security }
`init` 함수에 비밀번호를 전달하여 저장소에 있는 모든 콜렉션을 보호할 수 있습니다. 비밀번호가 전달되지 않으면 저장소에 있는 모든 콜렉션의 문서가 암호화되지 않습니다.

데이터 암호화는 Android, iOS, Windows 8.1 Universal 및 Windows 10 UWP 환경에서만 사용 가능합니다.  
일부 보안 메타데이터는 *키 체인*(iOS), *공유 환경 설정*(Android) 또는 *인증 정보 보관*(Windows 8.1)에 저장됩니다.  
저장소는 256비트 고급 암호화 표준(AES) 키로 암호화됩니다. 모든 키는 PBKDF2(Password-Based Key Derivation Function 2)로 강화됩니다.

`init`를 다시 호출할 때까지 모든 콜렉션에 대한 액세스를 잠그려면 `closeAll`을 사용하십시오. `init`를 로그인 함수로 고려하는 경우 `closeAll`을 해당 로그아웃 함수로 고려할 수 있습니다. 비밀번호를 변경하려면 `changePassword`를 사용하십시오.

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

#### 암호화
{: #encryption }
*iOS만 해당*됩니다. 기본적으로 iOS용 MobileFirst Cordova SDK는 iOS 제공 API를 암호화에 사용합니다. 이를 OpenSSL로 대체하려면 다음을 수행하십시오.

1. cordova-plugin-mfp-encrypt-utils 플러그인을 추가하십시오. `cordova plugin add cordova-plugin-mfp-encrypt-utils`
2. 애플리케이션 로직에서 `WL.SecurityUtils.enableNativeEncryption(false)`을 사용하여 OpenSSL 옵션을 사용으로 설정하십시오.

### 다중 사용자 지원
{: #multiple-user-support }
단일 MobileFirst 애플리케이션에 여러 콜렉션을 포함하는 여러 저장소를 작성할 수 있습니다.`init` 함수는 사용자 이름을 사용하여 옵션 오브젝트를 가져올 수 있습니다. 사용자 이름이 제공되지 않는 경우 기본 사용자 이름은 **jsonstore**입니다.

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

### MobileFirst 어댑터 통합
{: #mobilefirst-adapter-integration }
이 절에서는 사용자가 어댑터에 익숙하다고 가정합니다.  

어댑터 통합은 선택사항이며 콜렉션의 데이터를 어댑터로 전송하고 어댑터의 데이터를 콜렉션으로 가져오는 방법을 제공합니다.  
보다 유연해야 하는 경우 `WLResourceRequest` 또는 `jQuery.ajax`를 사용하여 이러한 목표를 달성할 수 있습니다.

### 어댑터 구현
{: #adapter-implementation }
어댑터를 작성하고 이름을 "**JSONStoreAdapter**"로 지정하십시오.  
해당 프로시저 `addPerson`, `getPeople`, `pushPeople`, `removePerson` 및 `replacePerson`을 정의하십시오.

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


#### MobileFirst 어댑터에서 데이터 로드
{: #load-data-from-an-adapter }
어댑터에서 데이터를 로드하려면 `WLResourceRequest`를 사용하십시오.

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

#### 푸시 가져오기 필요(더티 문서)
{: #get-push-required-dirty-documents }
`getPushRequired`를 호출하면 백엔드 시스템에 존재하지 않는 로컬 수정사항이 있는 문서인 *"더티 문서"*라는 배열이 리턴됩니다. 이러한 문서는 `push` 호출 시 어댑터에 전송됩니다.

```javascript
var collectionName = 'people';
WL.JSONStore.get(collectionName).getPushRequired().then(function (dirtyDocuments) {
    // handle success
}).fail(function (error) {
  // handle failure
});
```
{: codeblock}

JSONStore에서 문서를 "더티"로 표시하지 않게 하려면 `{markDirty:false}` 옵션을 `add`, `replace` 및 `remove`에 전달하십시오.

`getAllDirty` API를 사용하여 더티 문서를 검색할 수도 있습니다.

```javascript
WL.JSONStore.get(collectionName).getAllDirty()
.then(function (dirtyDocuments) {
    // handle success
}).fail(function (errorObject) {
    // handle failure
});
```
{: codeblock}

#### 변경사항 푸시
{: #push }
변경사항을 어댑터에 푸시하려면 `getAllDirty`를 호출하여 수정된 문서 목록을 가져온 후 `WLResourceRequest`를 사용하십시오. 데이터가 전송되고 성공적인 응답이 수신된 후 `markClean`을 호출해야 합니다.

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

### 확장
{: #enhance }
`enhance`를 통해 콜렉션 프로토타입에 함수를 추가하여 요구사항을 맞게 코어 API를 확장하십시오.
다음 예제(아래의 코드 스니펫)는 `enhance`를 사용하여 `keyvalue` 콜렉션에서 작동하는 `getValue` 함수를 추가하는 방법을 보여줍니다. 이는 `key`(문자열)를 유일한 매개변수로 사용하고 하나의 결과를 리턴합니다.

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

## 샘플 애플리케이션
{: #sample-application }
JSONStoreSwift 프로젝트에는 JSONStore API 세트를 활용하는 Cordova 애플리케이션이 있습니다.  
JavaScript 어댑터 Maven 프로젝트가 포함되어 있습니다.

![JSONStore 샘플 앱](images/jsonstore-cordova.png)

Cordova 프로젝트를 [다운로드하려면 클릭](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreCordova/tree/release80)하십시오.  
어댑터 Maven 프로젝트를 [다운로드하려면 클릭](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreAdapter/tree/release80)하십시오.  

### 샘플 사용법
{: #sample-usage }
샘플의 README.md 파일에 있는 지시사항에 따르십시오.
