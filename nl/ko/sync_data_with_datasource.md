---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: synchronization of data, sync with offline storage, jsonstore sync

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# 데이터 소스와 데이터 동기화
{: #sync_data_with_datasource}

[Cloudant®](https://www.ibm.com/in-en/marketplace/database-management)를 포함하여 디바이스의 JSONStore 콜렉션과 원격 CouchDB 데이터베이스 간에 데이터를 자동으로 동기화할 수 있습니다.

## JSONStore 및 Cloudant 간의 동기화 설정
{: #set_up_sync}

JSONStore 및 Cloudant 간의 자동 동기화를 설정하려면 다음 단계를 완료하십시오.

1. 모바일 앱에서 **동기화 정책**을 정의하십시오.
2. Mobile Foundation에서 **동기화 어댑터**를 배치하십시오.

### 동기화 정책 정의
{: #define_sync_policy}

JSONStore 콜렉션 및 Cloudant 데이터베이스 간의 동기화 방법은 **동기화 정책**으로 정의됩니다. 각 콜렉션에 대한 앱에서 **동기화 정책**을 지정하십시오.
JSONStore 콜렉션은 **동기화 정책** 필드로 초기화되어야 합니다. **동기화 정책**은 다음 세 개의 정책 중 하나일 수 있습니다.

1. `SYNC_DOWNSTREAM`
   Cloudant에서 JSONStore 콜렉션으로 데이터를 다운로드하려는 경우 `SYNC_DOWNSTREAM` 정책을 사용하십시오. 일반적으로 이 정책은 오프라인 스토리지에 필요한 정적 데이터에 사용됩니다 (예: 카탈로그에 있는 항목의 가격 목록). 디바이스에서 콜렉션이 초기화될 때마다 원격 Couldant 데이터베이스에서 데이터를 새로 고칩니다. 처음에는 전체 데이터베이스가 다운로드되지만 다음 새로 고치기 시에는 원격 데이터베이스에서 작성된 변경사항으로 구성된 델타만 다운로드됩니다.
  
다음 사용법을 검토하십시오.

   * Android:
  
   ```java
  initOptions.setSyncPolicy(JSONStoreSyncPolicy.SYNC_DOWNSTREAM);
   ```

   * iOS: 
  
   ```objc
  openOptions.syncPolicy = SYNC_DOWNSTREAM;
   ```

   * Cordova: 
  
   ```javascript
  collection.sync = {
     syncPolicy:WL.JSONStore.syncOptions.SYNC_DOWNSTREAM
  }
   ```

2. `SYNC_UPSTREAM`
  Cloudant 데이터베이스로 로컬 데이터를 푸시하려면 이 정책을 사용하십시오. 예를 들어, Cloudant 데이터베이스에 오프라인으로 캡처된 판매 데이터를 업로드하는 경우입니다. 콜렉션이 `SYNC_UPSTREAM` 정책으로 정의된 경우 콜렉션에 새 레코드가 추가되면 Cloudant에 새 레코드가 작성됩니다. 마찬가지로, 디바이스의 콜렉션에서 문서가 수정되면 Cloudant의 문서가 수정되고 콜렉션에서 문서가 삭제되면 Cloudant 데이터베이스에서도 삭제됩니다.

다음 사용법을 검토하십시오.

   * Android:
   ```java
  initOptions.setSyncPolicy(JSONStoreSyncPolicy.SYNC_UPSTREAM);
   ```

   * iOS:
   ```objc
  openOptions.syncPolicy = SYNC_UPSTREAM;
   ```

   * Cordova:
   ```javascript
  collection.sync = {
     syncPolicy:WL.JSONStore.syncOptions.SYNC_UPSTREAM
  }
   ```

3. `SYNC_NONE`
  `SYNC_NONE`은 기본 정책입니다. 동기화가 발생하지 않도록 하려면 이 정책을 선택하십시오.

**동기화 정책** JSONStore 콜렉션에 기인합니다. 콜렉션이 특정 **동기화 정책**으로 초기화되는 경우 변경되어서는 안됩니다. **동기화 정책**을 수정하면 원하지 않는 결과가 발생할 수 있습니다.

### 동기화 어댑터 배치
{: #deploy_sync_adapter}

`syncAdapterPath`
이 구성은 배치되는 어댑터 이름을 가져옵니다.

다음 사용법을 검토하십시오.

   * Android:
   ```java
 initOptions.syncAdapterPath = "JSONStoreCloudantSync"; //Here "JSONStoreCloudantSync" is the name of the adapter.
   ```

   * iOS:
   ```objc
  openOptions.syncAdapterPath = @"JSONStoreCloudantSync";
   ```

   * Cordova 또는 Ionic:
   ```javascript
  collection.sync = {
    syncAdapterPath:"JSONStoreCloudantSync"
  }
   ```

[여기](https://github.com/MobileFirst-Platform-Developer-Center/JSONStoreCloudantSync/)에서 `JSONStoreSync` 어댑터를 다운로드하고 `src/main/adapter-resources/adapter.xml` 경로에서 Cloudant 인증 정보를 구성하여 Mobile Foundation 서버에 배치하십시오.

Mobile Foundation Operations 콘솔에서 백엔드 Cloudant 데이터베이스에 대한 인증 정보를 구성하십시오.

### 수동으로 동기화 오퍼레이션 수행
{: #performing_sync_manual}

초기화 이후 언제든지 명시적으로 업스트림 또는 다운스트림 동기화가 수행되어야 하는 경우 다음 API를 사용할 수 있습니다.

`sync()`

이 API는 호출 콜렉션의 동기화 정책이 `SYNC_DOWNSTREAM`으로 설정된 경우 다운스트림 동기화를 수행합니다. 동기화 정책이 `SYNC_UPSTREAM`으로 설정되면 JSONStore에서 Cloudant 데이터베이스로의 업스트림 동기화가 수행됩니다. 추가, 삭제 또는 대체된 문서에 대한 동기화가 수행됩니다.

다음 사용법을 검토하십시오. 

  * Android:
 ```java
 WLJSONStore.getInstance(context).getCollectionByName(collection_name).sync();
 ```

  * iOS:
 ```objc
  collection.sync(); //Here collection is the JSONStore collection object that was initialized
 ```

  * Cordova:
 ```javascript
  WL.JSONStore.get(collectionName).sync();
 ```
