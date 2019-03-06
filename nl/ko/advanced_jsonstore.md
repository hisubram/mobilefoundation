---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-12"

---
{:generic: .ph data-hd-programlang='generic'}
{:java: .ph data-hd-programlang='java'}
{:ruby: .ph data-hd-programlang='ruby'}
{:c#: .ph data-hd-programlang='c#'}
{:objectc: .ph data-hd-programlang='Objective C'}
{:python: .ph data-hd-programlang='python'}
{:javascript: .ph data-hd-programlang='javascript'}
{:php: .ph data-hd-programlang='PHP'}
{:swift: .ph data-hd-programlang='swift'}
{:curl: .ph data-hd-programlang='curl'}
{:generic: .ph data-hd-operatingsystem='generic'}
{:ios: .ph data-hd-programlang='iOS'}
{:android: .ph data-hd-programlang='Android'}
{:cordova: .ph data-hd-programlang='Cordova'}
{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# 고급 JSONStore 
{: #advanced_jsonstore}

## JSONStore의 보안
{: #security_jsonstore}

<!--### Cordova
{: #security_jsonstore_cordova}-->

`init` 함수에 비밀번호를 전달하여 저장소에 있는 모든 콜렉션을 보호할 수 있습니다. 비밀번호가 전달되지 않으면 저장소에 있는 모든 콜렉션의 문서가 암호화되지 않습니다.
{: cordova}

데이터 암호화는 Android, iOS, Windows 8.1 Universal 및 Windows 10 UWP 환경에서만 사용 가능합니다.
일부 보안 메타데이터는 키 체인(iOS), 공유 환경 설정(Android) 또는 인증 정보 보관(Windows 8.1)에 저장됩니다.
저장소는 256비트 고급 암호화 표준(AES) 키로 암호화됩니다. 모든 키는 PBKDF2(Password-Based Key Derivation Function 2)로 강화됩니다.
{: cordova}

`init`를 다시 호출할 때까지 모든 콜렉션에 대한 액세스를 잠그려면 `closeAll`을 사용하십시오. `init`를 로그인 함수로 고려하는 경우 `closeAll`을 해당 로그아웃 함수로 고려할 수 있습니다. 비밀번호를 변경하려면 `changePassword`를 사용하십시오.
{: cordova}

암호화는 iOS에서만 지원됩니다. 기본적으로 iOS용 MobileFirst Cordova SDK는 iOS 제공 API를 암호화에 사용합니다. 이를 OpenSSL로 대체하려면 다음을 수행하십시오.
{: cordova}

* `cordova-plugin-mfp-encrypt-utils` 플러그인을 추가하십시오. 
  ```bash
  cordova plugin add cordova-plugin-mfp-encrypt-utils.
  ```
* 애플리케이션 로직에서 `WL.SecurityUtils.enableNativeEncryption(false)`을 사용하여 OpenSSL 옵션을 사용으로 설정하십시오.
{: cordova}

<!--### iOS
{: #security_jsonstore_ios} -->

`JSONStoreOpenOptions` 오브젝트를 비밀번호와 함께 `openCollections` 함수에 전달하여 저장소의 모든 콜렉션을 보호할 수 있습니다. 비밀번호가 전달되지 않으면 저장소에 있는 모든 콜렉션의 문서가 암호화되지 않습니다.
{: ios}

일부 보안 메타데이터는 키 체인에 저장됩니다(iOS).
저장소는 256비트 고급 암호화 표준(AES) 키로 암호화됩니다. 모든 키는 PBKDF2(Password-Based Key Derivation Function 2)로 강화됩니다.
{: ios}

`openCollections`를 다시 호출할 때까지 모든 콜렉션에 대한 액세스를 잠그려면 `closeAllCollections`를 사용하십시오. `openCollections`를 로그인 함수로 고려하는 경우 `closeAllCollections`를 해당 로그아웃 함수로 고려할 수 있습니다.
{: ios}

비밀번호를 변경하려면 `changeCurrentPassword`를 사용하십시오.
{: ios}

```swift
let collection:JSONStoreCollection = JSONStoreCollection(name: "people")
collection.setSearchField("name", withType: JSONStore_String)
collection.setSearchField("age", withType: JSONStore_Integer)

let options:JSONStoreOpenOptions = JSONStoreOpenOptions()
options.password = "123"

do {
  try JSONStore.sharedInstance().openCollections([collection], withOptions: options)
} catch let error as NSError {
  // handle error
}
```
{: codeblock}
{: ios}

<!--### Android
{: #security_jsonstore_android} -->

`JSONStoreInitOptions` 오브젝트를 비밀번호와 함께 `openCollections` 함수에 전달하여 저장소의 모든 콜렉션을 보호할 수 있습니다. 비밀번호가 전달되지 않으면 저장소에 있는 모든 콜렉션의 문서가 암호화되지 않습니다.
{: android}

일부 보안 메타데이터는 공유 환경 설정(Android)에 저장됩니다.
저장소는 256비트 고급 암호화 표준(AES) 키로 암호화됩니다. 모든 키는 PBKDF2(Password-Based Key Derivation Function 2)로 강화됩니다.
{: android}

`openCollections`를 다시 호출할 때까지 모든 콜렉션에 대한 액세스를 잠그려면 `closeAllCollections`를 사용하십시오. `openCollections`를 로그인 함수로 고려하는 경우 `closeAllCollections`를 해당 로그아웃 함수로 고려할 수 있습니다.
{: android}

비밀번호를 변경하려면 `changeCurrentPassword`를 사용하십시오.
{: android}

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
{: android}

## JSONStore의 다중 사용자 지원
{: #multiple_user_jsonstore} 

<!--### Cordova
{: #multiple_user_jsonstore_cordova} -->

단일 MobileFirst 애플리케이션에 여러 콜렉션을 포함하는 다중 저장소를 작성할 수 있습니다. `init` 함수는 사용자 이름을 사용하여 옵션 오브젝트를 가져올 수 있습니다. 사용자 이름이 제공되지 않는 경우 기본 사용자 이름은 *jsonstore*입니다.
{: cordova}

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
{: cordova}

<!--### iOS
{: #multiple_user_jsonstore_ios} -->

단일 MobileFirst 애플리케이션에 여러 콜렉션을 포함하는 다중 저장소를 작성할 수 있습니다. `init` 함수는 사용자 이름을 사용하여 옵션 오브젝트를 가져올 수 있습니다. 사용자 이름이 제공되지 않는 경우 기본 사용자 이름은 *jsonstore*입니다.
{: ios}

```swift
let collection:JSONStoreCollection = JSONStoreCollection(name: "people")
collection.setSearchField("name", withType: JSONStore_String)
collection.setSearchField("age", withType: JSONStore_Integer)

let options:JSONStoreOpenOptions = JSONStoreOpenOptions()
options.username = "yoel"

do {
  try JSONStore.sharedInstance().openCollections([collection], withOptions: options)
} catch let error as NSError {
  // handle error
}
```
{: codeblock}
{: ios}

<!--### Android
{: #multiple_user_jsonstore_android} -->

단일 Mobile Foundation 애플리케이션에 여러 콜렉션을 포함하는 다중 저장소를 작성할 수 있습니다. `openCollections` 함수는 사용자 이름을 사용하여 옵션 오브젝트를 가져올 수있습니다. 사용자 이름이 제공되지 않는 경우 기본 사용자 이름은 *jsonstore*입니다.
{: android}

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
{: android}

## 어댑터 통합
{: #adapter_integration}

<!--### Cordova
{: #adapter_integration_cordova}-->

어댑터 통합은 선택사항이며 콜렉션의 데이터를 어댑터로 전송하고 어댑터의 데이터를 콜렉션으로 가져오는 방법을 제공합니다.
보다 유연해야 하는 경우 `WLResourceRequest` 또는 `jQuery.ajax`를 사용하여 이러한 목표를 달성할 수 있습니다.
{: cordova}

1. 어댑터를 작성하고 이름을 **JSONStoreAdapter**로 지정하십시오.
2. 해당 프로시저 `addPerson`, `getPeople`, `pushPeople`, `removePerson` 및 `replacePerson`을 정의하십시오.
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
   {: cordova}
3. 어댑터에서 데이터를 로드하려면 `WLResourceRequest`를 사용하십시오.
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
   {: cordova}   
4. `getPushRequired`를 호출하면 백엔드 시스템에 존재하지 않는 로컬 수정사항이 있는 문서인 "더티 문서"라는 배열이 리턴됩니다. 이러한 문서는 `push` 호출 시 어댑터에 전송됩니다.
   ```javascript
   var collectionName = 'people';
   WL.JSONStore.get(collectionName).getPushRequired().then(function (dirtyDocuments) {
        // handle success
   }).fail(function (error) {
      // handle failure
   });
   ```
   {: codeblock}
   {: cordova}
   JSONStore에서 문서를 "더티"로 표시하지 않게 하려면 `{markDirty:false}` 옵션을 `add`, `replace` 및 `remove`에 전달하십시오.
   {: tip} 
   {: cordova}
5. `getAllDirty` API를 사용하여 더티 문서를 검색할 수도 있습니다.
   ```javascript
   WL.JSONStore.get(collectionName).getAllDirty()
    .then(function (dirtyDocuments) {
        // handle success
    }).fail(function (errorObject) {
        // handle failure
    });
   ```
   {: codeblock}
   {: cordova}
6. 변경사항을 어댑터에 푸시하려면 `getAllDirty`를 호출하여 수정된 문서 목록을 가져온 후 `WLResourceRequest`를 사용하십시오. 데이터가 전송되고 성공적인 응답이 수신된 후 `markClean`을 호출해야 합니다.
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
   {: cordova}
7. `enhance`를 통해 콜렉션 프로토타입에 함수를 추가하여 요구사항을 맞게 코어 API를 확장하십시오. 다음 예제(아래의 코드 스니펫)는 `enhance`를 사용하여 `keyvalue` 콜렉션에서 작동하는 `getValue` 함수를 추가하는 방법을 보여줍니다. 이는 키((문자열)를 유일한 매개변수로 사용하고 하나의 결과를 리턴합니다.
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
   {: cordova}
8. **샘플** 절에서 Cordova 앱에 대한 JSONStore 샘플을 참조하십시오. 이 프로젝트에는 JSONStore API 세트를 사용하는 Cordova 애플리케이션이 포함되어 있습니다. [여기](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreAdapter/tree/release80)에서 JavaScript 어댑터 Maven 프로젝트를 다운로드할 수 있습니다.
{: cordova}

<!--### iOS
{: #adapter_integration_ios}-->

어댑터 통합은 선택사항이며 콜렉션의 데이터를 어댑터로 전송하고 어댑터의 데이터를 콜렉션으로 가져오는 방법을 제공합니다.
`WLResourceRequest`를 사용하여 이러한 목표를 달성할 수 있습니다.
{: ios}

1. 어댑터를 작성하고 이름을 **People**로 지정하십시오.
2. 해당 프로시저 `addPerson`, `getPeople`, `pushPeople`, `removePerson` 및 `replacePerson`을 정의하십시오.
3. 어댑터에서 데이터를 로드하려면 `WLResourceRequest`를 사용하십시오.
   ```swift
    // Start - LoadFromAdapter
    class LoadFromAdapter: NSObject, WLDelegate {
      func onSuccess(response: WLResponse!) {
        let responsePayload:NSDictionary = response.getResponseJson()
        let people:NSArray = responsePayload.objectForKey("peopleList") as! NSArray
        // handle success
      }

      func onFailure(response: WLFailResponse!) {
        // handle failure
      }
    }
    // End - LoadFromAdapter

    let pull = WLResourceRequest(URL: NSURL(string: "/adapters/People/getPeople"), method: "GET")

    let loadDelegate:LoadFromAdapter = LoadFromAdapter()
    pull.sendWithDelegate(loadDelegate)
   ```
   {: codeblock}
   {: ios}
4. `allDirty`를 호출하면 백엔드 시스템에 존재하지 않는 로컬 수정사항이 있는 문서인 "더티 문서"라는 배열이 리턴됩니다.
   ```swift
    let collectionName:String = "people"
    let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

    do {
      let dirtyDocs:NSArray = try collection.allDirty()
    } catch let error as NSError {
      // handle error
    }
   ```
   {: codeblock}
   {: ios}
   JSONStore에서 문서를 "더티"로 표시하지 않게 하려면 `{markDirty:false}` 옵션을 `add`, `replace` 및 `remove`에 전달하십시오.
   {: tip} 
5. 변경사항을 어댑터에 푸시하려면 `allDirty`를 호출하여 수정된 문서 목록을 가져온 후 `WLResourceRequest`를 사용하십시오. 데이터가 전송되고 성공적인 응답이 수신된 후 `markDocumentsClean`을 호출해야 합니다.
   ```swift
    // Start - PushToAdapter
    class PushToAdapter: NSObject, WLDelegate {
      func onSuccess(response: WLResponse!) {
        // handle success
      }

      func onFailure(response: WLFailResponse!) {
        // handle failure
      }
    }
    // End - PushToAdapter

    let collectionName:String = "people"
    let collection:JSONStoreCollection = JSONStore.sharedInstance().getCollectionWithName(collectionName)

    do {
      let dirtyDocs:NSArray = try collection.allDirty()
      let pushData:NSData = NSKeyedArchiver.archivedDataWithRootObject(dirtyDocs)

      let push = WLResourceRequest(URL: NSURL(string: "/adapters/People/pushPeople"), method: "POST")

      let pushDelegate:PushToAdapter = PushToAdapter()
      push.sendWithData(pushData, delegate: pushDelegate)

    } catch let error as NSError {
      // handle error
    }
   ```
   {: codeblock}
   {: ios}
6. **샘플** 절에서 네이티브 iOS Swift 애플리케이션 프로젝트를 다운로드하십시오. 이 프로젝트에는 JSONStore API 세트를 사용하는 네이티브 iOS Swift 애플리케이션이 포함되어 있습니다. [여기](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreAdapter/tree/release80)에서 JavaScript 어댑터 Maven 프로젝트를 다운로드할 수 있습니다.
{: ios}

<!--### Android
{: #adapter_integration_android}-->

어댑터 통합은 선택사항이며 콜렉션의 데이터를 어댑터로 전송하고 어댑터의 데이터를 콜렉션으로 가져오는 방법을 제공합니다.
보다 유연해야 하는 경우 `WLResourceRequest`와 같은 함수를 사용하거나 고유 `HttpClient` 인스턴스를 사용하여 이러한 목표를 달성할 수 있습니다.
{: android}

1. 어댑터를 작성하고 이름을 **JSONStoreAdapter**로 지정하십시오.
2. 해당 프로시저 `addPerson`, `getPeople`, `pushPeople`, `removePerson` 및 `replacePerson`을 정의하십시오.
3. 어댑터에서 데이터를 로드하려면 `WLResourceRequest`를 사용하십시오.
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
   {: android}
4. `findAllDirtyDocuments`를 호출하면 백엔드 시스템에 존재하지 않는 로컬 수정사항이 있는 문서인 "더티 문서"라는 배열이 리턴됩니다.
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
   {: android}
   JSONStore에서 문서를 "더티"로 표시하지 않게 하려면 `options.setMarkDirty(false)` 옵션을 `add`, `replace` 및 `remove`에 전달하십시오.
   {: tip} 
   {: android}
5. 변경사항을 어댑터에 푸시하려면 `findAllDirtyDocuments`를 호출하여 수정된 문서 목록을 가져온 후 `WLResourceRequest`를 사용하십시오. 데이터가 전송되고 성공적인 응답이 수신된 후 `markDocumentsClean`을 호출해야 합니다.
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
   {: android}
6. **샘플** 절에서 네이티브 Android 애플리케이션 프로젝트를 다운로드하십시오. 이 프로젝트에는 JSONStore API 세트를 사용하는 네이티브 Android 애플리케이션이 포함되어 있습니다. [여기](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreAdapter/tree/release80)에서 JavaScript 어댑터 Maven 프로젝트를 다운로드할 수 있습니다. 
{: android}
