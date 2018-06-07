---

copyright:
  years: 2018
lastupdated:  "2018-05-11"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

#	Cordova 애플리케이션에서 직접 업데이트
{: #direct_update_cordova_apps}

직접 업데이트를 사용하여 Cordova 애플리케이션의 웹 리소스(JavaScript, HTML, CSS 또는 이미지 파일)를 *OTA(Over-The-Air)*로 업데이트할 수 있습니다. 직접 업데이트 기능을 사용하면 비즈니스에서 일반 사용자가 최신 버전의 앱을 사용하는지 확인할 수 있습니다.
애플리케이션을 업데이트하려면 MobileFirst CLI를 사용하거나 생성된 아카이브 파일을 배치하여 애플리케이션의 업데이트된 웹 리소스를 패키지하고 MobileFirst Server에 업로드해야 합니다. 그러면 직접 업데이트가 자동으로 활성화됩니다. 이후 보호된 리소스에 대한 모든 요청에서 적용됩니다.

직접 업데이트는 Cordova iOS 및 Cordova Android 플랫폼에서 지원됩니다.

개발 및 테스트 용도의 경우 일반적으로 개발자가 단순히 개발 서버에 아카이브를 업로드하여 직접 업데이트를 사용합니다. 이 프로세스는 쉽게 구현할 수 있지만 안전하지는 않습니다. 이 단계에서는 임베디드 MobileFirst 자체 서명된 인증서에서 추출된 내부 RSA 키 쌍이 사용됩니다.

그러나 실시간 프로덕션 또는 사전 프로덕션 테스트 단계의 경우에는 애플리케이션을 앱 스토어에 공개하기 전에 보안 직접 업데이트를 구현하도록 권장됩니다. 보안 직접 업데이트를 사용하려면 실제 CA 서명 서버 인증서에서 추출된 RSA 키 쌍이 필요합니다.

* 애플리케이션이 공개된 후 키 저장소 구성을 수정하지 않도록 주의하십시오. 새 공개 키로 애플리케이션을 재구성하고 애플리케이션을 다시 공개하기 전에 다운로드된 업데이트를 인증할 수 없습니다. 이 두 개의 단계를 수행하지 않으면 클라이언트에서 직접 업데이트가 실패할 수 있습니다.
* 직접 업데이트는 애플리케이션의 웹 리소스만 업데이트합니다. 고유 리소스를 업데이트하려면 새 애플리케이션 버전을 각 앱 스토어에 제출해야 합니다.
* 직접 업데이트 기능을 사용하고 웹 리소스 체크섬 기능이 사용으로 설정되면 각 직접 업데이트에 대해 새 체크섬 기준이 설정됩니다.
* MobileFirst Server가 수정팩을 사용하여 업그레이드된 경우 직접 업데이트를 계속 올바르게 제공합니다. 그러나 최근에 빌드된 직접 업데이트 아카이브(.zip 파일)가 업로드되는 경우 이전 클라이언트에 대한 업데이트가 정지될 수 있습니다. 아카이브에 `cordova-plugin-mfp` 플러그인의 버전이 포함되어 있기 때문입니다. 서버가 해당 아카이브를 모바일 클라이언트에 제공하기 전에 클라이언트 버전을 플러그인 버전과 비교합니다. 두 버전이 충분히 비슷하면(즉, 가장 중요한 3개의 숫자가 동일한 경우) 직접 업데이트가 정상적으로 수행됩니다. 그렇지 않으면 MobileFirst Server가 업데이트를 자동으로 건너뜁니다. 버전 불일치에 대한 한 가지 솔루션은 원래 Cordova 프로젝트의 버전과 동일한 버전인 `cordova-plugin-mfp`를 다운로드하고 직접 업데이트 아카이브를 재생성하는 것입니다.
* 최적의 조건에서 단일 MobileFirst Server는 초당 250MB의 속도로 데이터를 클라이언트에 푸시할 수 있습니다. 더 빠른 속도가 필요한 경우 클러스터 또는 CDN 서비스를 고려하십시오.
{: tip}

## 직접 업데이트 작업 방식
{: #working_direct_update}

애플리케이션 웹 리소스는 최초 오프라인 가용성을 보장하기 위해 처음에 애플리케이션과 함께 패키지됩니다. 이후 애플리케이션이 MobileFirst Server에 대한 모든 요청의 업데이트가 있는지 확인합니다.

>**참고:** 직접 업데이트가 수행되면 60분 후에 다시 확인됩니다.

![직접 업데이트 작업 방식 다이어그램](images/internal_function.jpg)

직접 업데이트 후 애플리케이션에서 사전 패키지된 웹 리소스를 더 이상 사용하지 않습니다. 대신 애플리케이션의 샌드박스에서 다운로드된 웹 리소스를 사용합니다. 디바이스에 있는 애플리케이션의 캐시가 지워지면 원래 패키지된 웹 리소스가 다시 사용됩니다.

>직접 업데이트는 특정 버전에만 적용됩니다. 즉, 애플리케이션 버전 2.0에 대해 생성된 업데이트를 동일한 애플리케이션의 다른 버전에 적용할 수 없습니다.

## 업데이트된 웹 리소스 작성 및 배치
{: #creating_deploying_updates}

업데이트된 웹 리소스가 패키지되어 MobileFirst Server에 업로드되어야 합니다.

1. 명령행 창을 열고 Cordova 프로젝트의 루트로 이동하십시오.
2. 다음 명령을 실행하십시오.
  ```
  mfpdev app webupdate
  ```
  {: pre}
`mfpdev app webupdate` 명령은 업데이트된 웹 리소스를 `.zip` 파일로 패키지하여 개발자 워크스테이션에서 실행 중인 기본 MobileFirst Server에 업로드합니다. 패키지된 웹 리소스는 `[cordova-project-root-folder]/mobilefirst/` 폴더에 있습니다.

**대체 단계:**

* `.zip` 파일을 빌드하여 다른 MobileFirst Server에 업로드: `mfpdev app webupdate [server-name] [runtime-name]`. 예: 
  ```
  mfpdev app webupdate myQAServer MyBankApps
  ```
  {: pre}

* 이전에 생성된 `.zip` 파일 업로드: `mfpdev app webupdate [server-name] [runtime-name] --file [path-to-packaged-web-resources]`. 예: 
  ```
  mfpdev app webupdate myQAServer MyBankApps --file mobilefirst/ios/com.mfp.myBankApp-1.0.1.zip
  ```
  {: pre}

* 패키지된 웹 리소스를 수동으로 MobileFirst Server에 업로드:
  1. 업로드하지 않고 .zip 파일을 빌드하십시오.
      ```
      mfpdev app webupdate --build
      ```
      {: pre}
  2. MobileFirst Operations Console을 로드하고 애플리케이션 항목을 클릭하십시오.
  3. **웹 리소스 파일 업로드**를 클릭하여 패키지된 웹 리소스를 업로드하십시오.    
      ![콘솔에서 직접 업데이트 .zip 파일 업로드](images/upload-direct-update-package.png)

자세히 알아보려면 `mfpdev help app webupdate` 명령을 실행하십시오.
{: tip}

## 사용자 경험
{: #user_experience}

기본적으로 직접 업데이트가 수신된 후 대화 상자가 표시되고 사용자에게 업데이트 프로세스를 시작할 수 있는 권한을 요청합니다. 사용자가 승인하면 진행 표시줄 대화 상자가 표시되고 웹 리소스가 다운로드됩니다. 업데이트가 완료된 후 애플리케이션이 자동으로 다시 로드됩니다.

![직접 업데이트 예제](images/direct-update-flow.png)

## 직접 업데이트 UI 사용자 정의
{: #customize_du_ui}

일반 사용자에게 표시되는 기본 직접 업데이트 UI를 사용자 정의할 수 있습니다.
**index.js**의 `wlCommonInit()` 함수 내부에 다음을 추가하십시오.
```JavaScript
wl_DirectUpdateChallengeHandler.handleDirectUpdate = function(directUpdateData, directUpdateContext) {
    // Implement custom Direct Update logic
};
```
{: codeblock}

*directUpdateData*는 MobileFirst Server에서 다운로드될 업데이트 패키지의 파일 크기(바이트)를 표시하는 downloadSize 특성이 포함된 JSON 오브젝트입니다.
*directUpdateContext*는 직접 업데이트 플로우를 시작 및 중지하는 .start() 및 .stop() 함수를 공개하는 JavaScript 오브젝트입니다.

애플리케이션보다 MobileFirst Server의 웹 리소스가 최신인 경우 직접 업데이트 인증 확인 데이터가 서버 응답에 추가됩니다. MobileFirst 클라이언트 측 프레임워크가 이 직접 업데이트 인증 확인을 발견할 때마다 `wl_directUpdateChallengeHandler.handleDirectUpdate` 함수를 호출합니다.

이 함수는 기본 직접 업데이트 디자인(직접 업데이트가 사용 가능한 경우 표시되는 기본 메시지 대화 상자 및 직접 업데이트 프로세스가 시작될 때 표시되는 기본 진행상태 화면)을 제공합니다. 사용자 정의 직접 업데이트 사용자 인터페이스 동작을 구현하거나 이 함수를 대체하고 고유 로직을 구현하여 직접 업데이트 대화 상자를 사용자 정의할 수 있습니다.

아래 예제 코드에서 `handleDirectUpdate` 함수는 직접 업데이트 대화 상자에서 사용자 정의 메시지를 구현합니다. Cordova 프로젝트의 `www/js/index.js` 파일에 이 코드를 추가하십시오.
사용자 정의된 직접 업데이트 UI의 추가 예제는 다음과 같습니다.
* 써드파티 JavaScript 프레임워크(예: Dojo 또는 jQuery Mobile, Ionic 등)를 사용하여 작성되는 대화 상자
* Cordova 플러그인 실행을 통한 완전한 고유 UI
* 옵션과 함께 사용자에게 표시되는 대체 HTML 등

```JavaScript
wl_directUpdateChallengeHandler.handleDirectUpdate = function(directUpdateData, directUpdateContext) {        
    navigator.notification.confirm(  // Creates a dialog.
        'Custom dialog body text',
        // Handle dialog buttons.
          directUpdateContext.start();
        },
        'Custom dialog title text',
        ['Update']
    );
};
```
{: codeblock}

사용자가 대화 상자 단추를 클릭할 때마다 `directUpdateContext.start()` 메소드를 실행하여 직접 업데이트 프로세스를 시작할 수 있습니다. 이전 버전의 MobileFirst Server에 포함된 것과 유사한 기본 진행상태 화면이 표시됩니다.

이 메소드는 다음과 같은 유형의 호출을 지원합니다.
* 매개변수가 지정되지 않으면 MobileFirst Server에서 기본 진행상태 화면을 사용합니다.
* `directUpdateContext.start(directUpdateCustomListener)`와 같은 리스너 함수가 제공되면 프로세스가 리스너에 라이프사이클 이벤트를 전송하는 동안 직접 업데이트 프로세스가 백그라운드에서 실행됩니다. 사용자 정의 리스너는 다음 메소드를 구현해야 합니다.

```JavaScript
var  directUpdateCustomListener  = {
    onStart : function ( totalSize ){ },
    onProgress : function ( status , totalSize , completedSize ){ },
    onFinish : function ( status ){ }
};
```
{: codeblock}

리스너 메소드는 다음 규칙에 따라 직접 업데이트 프로세스 중에 시작됩니다.
* `onStart`는 업데이트 파일 크기를 보유하는 `totalSize` 매개변수와 함께 호출됩니다.
* `onProgress`는 `DOWNLOAD_IN_PROGRESS`, `totalSize` 및 `completedSize`(현재까지 다운로드된 볼륨) 상태에서 여러 번 호출됩니다.
* `onProgress`는 `UNZIP_IN_PROGRESS` 상태에서 호출됩니다.
* `onFinish`는 다음 최종 상태 코드 중 하나로 호출됩니다.

| 상태 코드 | 설명 |
|:------------|:------------|
| `SUCCESS` | 직접 업데이트가 오류 없이 완료되었습니다. |
| `CANCELED` | 직접 업데이트가 취소되었습니다(예를 들어, `stop()` 메소드가 호출되었기 때문에). |
| `FAILURE_NETWORK_PROBLEM` |업데이트 중에 네트워크 연결 문제점이 발생했습니다. |
| `FAILURE_DOWNLOADING` | 파일이 완전히 다운로드되지 않았습니다. |
| `FAILURE_NOT_ENOUGH_SPACE` | 업데이트 파일을 다운로드하고 압축을 풀기 위한 충분한 공간이 디바이스에 없습니다. |
| `FAILURE_UNZIPPING` | 업데이트 파일의 압축을 푸는 중에 문제점이 발생했습니다. |
| `FAILURE_ALREADY_IN_PROGRESS` | 직접 업데이트 실행 중에 시작 메소드가 호출되었습니다. |
| `FAILURE_INTEGRITY` | 업데이트 파일의 진위성을 확인할 수 없습니다.|
| `FAILURE_UNKNOWN` | 예상치 못한 내부 오류가 발생했습니다. |

사용자 정의 직접 업데이트 리스너를 구현하는 경우 직접 업데이트 프로세스가 완료되고 `onFinish()` 메소드가 호출되었을 때 앱이 다시 로드되는지 확인해야 합니다. 또한 직접 업데이트 프로세스를 성공적으로 완료하지 못한 경우 `wl_directUpdateChalengeHandler.submitFailure()`를 호출해야 합니다.

다음 예는 사용자 정의 직접 업데이트 리스너 구현을 보여줍니다.
```JavaScript
var directUpdateCustomListener = {
  onStart: function(totalSize){
    //show custom progress dialog
  },
  onProgress: function(status,totalSize,completedSize){
    //update custom progress dialog
  },
  onFinish: function(status){

    if (status == 'SUCCESS'){
      //show success message
      WL.Client.reloadApp();
    }
    else {
      //show custom error message

      //submitFailure must be called is case of error
      wl_directUpdateChallengeHandler.submitFailure();
    }
  }
};

wl_directUpdateChallengeHandler.handleDirectUpdate = function(directUpdateData, directUpdateContext){

  WL.SimpleDialog.show('Update Avalible', 'Press update button to download version 2.0', [{
    text : 'update',
    handler : function() {
      directUpdateContext.start(directUpdateCustomListener);
    }
  }]);
};
```
{: codeblock}

### 시나리오: UI가 없는 직접 업데이트 실행
{: #scenario-running-ui-less-direct-updates }
{{site.data.keyword.mobilefoundation_short}}은 애플리케이션이 포그라운드에 있을 때 UI가 없는 직접 업데이트를 지원합니다.

UI가 없는 직접 업데이트를 실행하려면 `directUpdateCustomListener`를 구현하십시오. `onStart` 및 `onProgress` 메소드에 빈 함수 구현을 제공하십시오. 구현이 비어 있으면 직접 업데이트 프로세스가 백그라운드에서 실행됩니다.

직접 업데이트 프로세스를 완료하려면 애플리케이션이 다시 로드되어야 합니다. 사용할 수 있는 옵션은 다음과 같습니다.
* `onFinish` 메소드도 비어 있을 수 있습니다. 이 경우 직접 업데이트는 애플리케이션이 다시 시작된 후에 적용됩니다.
* 사용자에게 애플리케이션을 다시 시작하도록 알리거나 요구하는 사용자 정의 대화 상자를 구현할 수 있습니다(다음 예제 참조).
* `onFinish` 메소드는 `WL.Client.reloadApp()`을 호출하여 애플리케이션 다시 로드를 강제 실행할 수 있습니다. 

다음은 `directUpdateCustomListener` 구현의 예입니다. 

```JavaScript
var directUpdateCustomListener = {
  onStart: function(totalSize){
  },
  onProgress: function(status,totalSize,completeSize){
  },
  onFinish: function(status){
    WL.SimpleDialog.show('New Update Available', 'Press reload button to update to new version', [ {
      text : WL.ClientMessages.reload,
      handler : WL.Client.reloadApp
    }]);
  }
};
```
{: codeblock}

`wl_directUpdateChallengeHandler.handleDirectUpdate` 함수를 구현하십시오. 함수에 대한 매개변수로 작성한 `directUpdateCustomListener` 구현을 전달하십시오. `directUpdateContext.start(directUpdateCustomListener)</codeph>가 호출되는지 확인하십시오. 다음은 `wl_directUpdateChallengeHandler.handleDirectUpdate` 구현의 예입니다.

```JavaScript
wl_directUpdateChallengeHandler.handleDirectUpdate = function(directUpdateData, directUpdateContext){

  directUpdateContext.start(directUpdateCustomListener);
};
```
{: codeblock}

**참고:** 애플리케이션이 백그라운드로 전송되면 직접 업데이트 프로세스가 일시중단됩니다.

### 시나리오: 직접 업데이트 실패 처리
{: #scenario-handling-a-direct-update-failure }
이 시나리오는 예를 들어 연결 유실로 인해 발생할 수 있는 직접 업데이트 실패를 처리하는 방법을 보여줍니다. 이 시나리오에서는 사용자가 오프라인 모드에서 앱을 사용할 수 없습니다. 사용자에게 다시 시도하는 옵션을 제공하는 대화 상자가 표시됩니다.

나중에 직접 업데이트 프로세스가 실패하는 경우 사용할 수 있도록 직접 업데이트 컨텍스트를 저장할 글로벌 변수를 작성하십시오. 예를 들어, 다음과 같습니다.

```JavaScript
var savedDirectUpdateContext;
```
{: codeblock}

직접 업데이트 인증 확인 핸들러를 구현하십시오. 여기에 직접 업데이트 컨텍스트를 저장하십시오. 예를 들어, 다음과 같습니다.

```JavaScript
wl_directUpdateChallengeHandler.handleDirectUpdate = function(directUpdateData, directUpdateContext){

  savedDirectUpdateContext = directUpdateContext; // save direct update context

  var downloadSizeInMB = (directUpdateData.downloadSize / 1048576).toFixed(1).replace(".", WL.App.getDecimalSeparator());
  var directUpdateMsg = WL.Utils.formatString(WL.ClientMessages.directUpdateNotificationMessage, downloadSizeInMB);

  WL.SimpleDialog.show(WL.ClientMessages.directUpdateNotificationTitle, directUpdateMsg, [{
    text : WL.ClientMessages.update,
    handler : function() {
      directUpdateContext.start(directUpdateCustomListener);
    }
  }]);
};
```
{: codeblock}

직접 업데이트 컨텍스트를 사용하여 직접 업데이트 프로세스를 시작하는 함수를 작성하십시오. 예를 들어, 다음과 같습니다.

```JavaScript
restartDirectUpdate = function () {
  savedDirectUpdateContext.start(directUpdateCustomListener); // use saved direct update context to restart direct update
};
```
{: codeblock}

`directUpdateCustomListener`를 구현하십시오. `onFinish` 메소드에 상태 확인을 추가하십시오. 상태가 `FAILURE`로 시작하는 경우 **다시 시도** 옵션을 사용하여 모달 전용 대화 상자를 여십시오. 예를 들어, 다음과 같습니다.

```JavaScript
var directUpdateCustomListener = {
  onStart: function(totalSize){
    alert('onStart: totalSize = ' + totalSize + 'Byte');
  },
  onProgress: function(status,totalSize,completeSize){
    alert('onProgress: status = ' + status + ' completeSize = ' + completeSize + 'Byte');
  },
  onFinish: function(status){
    alert('onFinish: status = ' + status);
    var pos = status.indexOf("FAILURE");
    if (pos > -1) {
      WL.SimpleDialog.show('Update Failed', 'Press try again button', [ {
        text : "Try Again",
        handler : restartDirectUpdate // restart direct update
      }]);
    }
  }
};
```
{: codeblock}

사용자가 **다시 시도** 단추를 클릭하면 애플리케이션에서 직접 업데이트 프로세스를 다시 시작합니다.

## 델타 및 전체 직접 업데이트
{: #delta-and-full-direct-update }
델타 직접 업데이트를 사용하면 애플리케이션이 애플리케이션의 전체 웹 리소스 대신 마지막 업데이트 이후 변경된 파일만 다운로드할 수 있습니다. 이렇게 하면 다운로드 시간이 감소하고 대역폭이 유지되며 전반적인 사용자 경험이 개선됩니다.

> <span class="glyphicon glyphicon-exclamation-sign" aria-hidden="true"></span> **중요:** **델타 업데이트**는 클라이언트 애플리케이션의 웹 리소스가 현재 서버에 배치된 애플리케이션보다 한 버전 이전인 경우에만 수행할 수 있습니다. 현재 배치된 애플리케이션 버전보다 두 버전 이상 전인 클라이언트 애플리케이션(즉, 클라이언트 애플리케이션이 업데이트된 후 애플리케이션이 두 번 이상 서버에 배치됨)은 **전체 업데이트**(즉, 전체 웹 리소스가 다운로드되고 업데이트됨)를 수신합니다.


## 샘플 애플리케이션
{: #sample-application }
Cordova 프로젝트를 [다운로드하려면 클릭 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/MobileFirst-Platform-Developer-Center/CustomDirectUpdate/tree/release80)하십시오.  

### 샘플 사용법
{: #sample-usage }
샘플의 README.md 파일에 있는 지시사항에 따르십시오.

## CDN 지원
{: #cdn_support}

MobileFirst Server 대신 CDN(Content Delivery Network)에서 직접 업데이트 요청을 제공하도록 구성할 수 있습니다.

### CDN 사용의 장점
{: #advantages-of-using-a-cdn }
MobileFirst Server 대신 CDN을 사용하여 직접 업데이트 요청을 제공하는 경우 다음과 같은 장점이 있습니다.

* MobileFirst Server에서 네트워크 오버헤드를 제거합니다.
* MobileFirst Server에서 요청을 제공할 때 250MB/초 한계보다 높게 전송률을 늘립니다.
* 지리적 위치에 구애받지 않고 모든 사용자에게 한층 일관된 직접 업데이트 경험을 보장합니다.

### 일반 요구사항
{: #general-requirements }
CDN에서 직접 업데이트 요청을 지원하려면 구성이 다음 조건을 준수하는지 확인하십시오.

* CDN이 MobileFirst Server의 앞에 있거나, 필요한 경우 다른 역방향 프록시의 앞에 있는 역방향 프록시여야 합니다.
* 개발 환경에서 애플리케이션을 빌드할 때 대상 서버를 MobileFirst Server의 호스트 및 포트 대신 CDN 호스트 및 포트로 설정하십시오. 예를 들어, MobileFirst CLI 명령 `mfpdev server add`를 실행하는 경우 CDN 호스트 및 포트를 제공하십시오.
* CDN 관리 패널에서 CDN이 직접 업데이트 요청을 제외한 모든 요청을 MobileFirst Server로 전달하도록 다음 직접 업데이트 URL을 캐싱 대상으로 표시해야 합니다. 직접 업데이트 요청의 경우 CDN이 컨텐츠를 확보했는지 여부를 판별합니다. 확보한 경우에는 MobileFirst Server로 이동하지 않고 컨텐츠를 리턴하며 그렇지 않은 경우에는 MobileFirst Server로 이동한 후 직접 업데이트 아카이브(.zip 파일)를 가져와서 해당 특정 URL에 대한 다음 요청을 위해 저장합니다. {{site.data.keyword.mobilefoundation_short}} v8.0으로 빌드된 애플리케이션의 경우 직접 업데이트 URL은 `PROTOCOL://DOMAIN:PORT/CONTEXT_PATH/api/directupdate/VERSION/CHECKSUM/TYPE`입니다.
`PROTOCOL://DOMAIN:PORT/CONTEXT_PATH` 접두부는 모든 런타임 요청에 대해 동일합니다. 예를 들어, `http://my.cdn.com:9080/mfp/api/directupdate/0.0.1/742914155/full?appId=com.ibm.DirectUpdateTestApp&clientPlatform=android`입니다.

이 예에는 요청의 일부이기도 한 추가 요청 매개변수가 있습니다.

* CDN이 요청 매개변수의 캐싱을 허용해야 합니다. 두 개의 서로 다른 직접 업데이트 아카이브는 요청 매개변수만 다릅니다.
* CDN이 직접 업데이트 응답에서 TTL을 지원해야 합니다. 동일한 버전에 대한 복수의 직접 업데이트를 지원하는 데 필요하기 때문입니다.
* CDN이 서버-클라이언트 프로토콜에서 사용되는 HTTP 헤더를 변경하거나 제거해서는 안됩니다.

### 예제 CDN 구성
{: #example-cdn-configuration }
이 예제는 직접 업데이트 아카이브를 캐시하는 Akamai CDN 구성 사용을 기반으로 합니다. 네트워크 관리자, MobileFirst 관리자 및 Akamai 관리자가 다음 태스크를 완료합니다.

#### 네트워크 관리자
{: #network-administrator }
MobileFirst Server에 대한 DNS에서 다른 도메인을 작성하십시오. 예를 들어, 서버 도메인이 `yourcompany.com`이면 `cdn.yourcompany.com`과 같은 추가 도메인을 작성해야 합니다.
새 `cdn.yourcompany.com` 도메인의 DNS에서 Akamai에서 제공되는 도메인 이름으로 `CNAME`을 설정하십시오(예: `yourcompany.com.akamai.net`).

#### MobileFirst 관리자
{: #mobilefirst-administrator }
새 `cdn.yourcompany.com` 도메인을 MobileFirst 애플리케이션의 MobileFirst Server URL로 설정하십시오. 예를 들어, Ant 빌더 태스크의 경우 특성은 다음과 같습니다.
```xml
<property name="wl.server" value="http://cdn.yourcompany.com/${contextPath}/"/>
```
{: codeblock}

#### Akamai 관리자
{: #akamai-administrator }
1. Akamai 특성 관리자를 열고 **호스트 이름** 특성을 새 도메인의 값으로 설정하십시오.

    ![호스트 이름 특성을 새 도메인의 값으로 설정](images/direct_update_cdn_3.jpg)

2. 기본 규칙 탭에서 원래 MobileFirst Server 호스트 및 포트를 구성하고 **사용자 정의 전달 호스트 헤더** 값을 새로 작성된 도메인으로 설정하십시오.

    ![사용자 정의 전달 호스트 헤더 값을 새로 정의된 도메인으로 설정](images/direct_update_cdn_4.jpg)

3. **캐싱 옵션** 목록에서 **저장소 없음**을 선택하십시오.

    ![캐싱 옵션 목록에서 저장소 없음 선택](images/direct_update_cdn_5.jpg)

4. **정적 컨텐츠 구성**  탭에서 애플리케이션의 직접 업데이트 URL에 따라 일치 기준을 구성하십시오. 예를 들어, `If Path matches one of direct_update_URL`로 명시하는 조건을 작성하십시오.

    ![애플리케이션의 직접 업데이트 URL에 따라 일치 기준 구성](images/direct_update_cdn_6.jpg)

5. 캐시 키의 모든 요청 매개변수를 사용하도록 캐시 키 동작을 구성하십시오(서로 다른 애플리케이션 또는 버전에 대해 서로 다른 직접 업데이트 아카이브를 캐시하려면 이를 수행해야 함). 예를 들어, **동작** 목록에서 `Include all parameters (preserve order from request)`를 선택하십시오.

    ![캐시 키의 모든 요청 매개변수를 사용하도록 캐시 키 동작 구성](images/direct_update_cdn_8.jpg)

6. 직접 업데이트 URL을 캐시하도록 캐싱 동작을 구성하고 TTL을 설정하려면 다음 값과 유사하게 설정하십시오.

      ![캐싱 동작을 구성하도록 값 설정](images/direct_update_cdn_7.jpg)

| 필드 | 값 |
|:------|:------|
| 캐싱 옵션 | 캐시 |
| 시간이 경과된(stale) 오브젝트의 강제 재평가 | 유효성 검증할 수 없는 경우 시간이 경과된(stale) 상태로 제공 |
| 최대 기간 | 3분 |


## 보안 직접 업데이트
{: #secure-dc }

기본적으로 사용 안함으로 설정되는 보안 직접 업데이트를 사용하면 써드파티 공격자가 MobileFirst Server 또는 CDN(Content Delivery Network)에서 클라이언트 애플리케이션으로 전송되는 웹 리소스를 변경하지 못합니다.

**직접 업데이트 진위성을 사용하려면 다음을 수행하십시오.**  
선호하는 도구를 사용하여 MobileFirst Server 키 저장소에서 공개 키를 추출하여 base64로 변환하십시오.  
생성되는 값은 아래에 지시된 대로 사용되어야 합니다.

1. **명령행** 창을 열고 Cordova 프로젝트의 루트로 이동하십시오.
2. `mfpdev app config` 명령을 실행하고 **직접 업데이트 진위성 공개 키** 옵션을 선택하십시오.
3. 공개 키를 제공하고 확인하십시오.

향후 클라이언트 애플리케이션에 대한 직접 업데이트 전달이 직접 업데이트 진위성으로 보호됩니다.

보안 직접 업데이트가 작동하려면 사용자 정의 키 저장소 파일이 MobileFirst Server에 배치되고 일치하는 공개 키의 사본이 배치된 클라이언트 애플리케이션에 포함되어 있어야 합니다.

이 주제에서는 공개 키를 새 클라이언트 애플리케이션 및 업그레이드된 기존 클라이언트 애플리케이션에 바인드하는 방법을 설명합니다. MobileFirst Server의 키 저장소 구성에 대한 자세한 정보는 [MobileFirst Server 키 저장소 구성 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/configuring-the-mobilefirst-server-keystore/){: new_window}을 참조하십시오.

서버는 개발 단계(Phase)를 위해 보안 직접 업데이트를 테스트하는 데 사용할 수 있는 기본 제공 키 저장소를 제공합니다.

>**참고:** 공개 키를 클라이언트 애플리케이션에 바인드하고 다시 빌드한 후에는 {{ site.data.keys.mf_server }}에 다시 업로드할 필요가 없습니다. 그러나 이전에 애플리케이션을 시장에 공개한 경우에는 공개 키 없이 다시 공개해야 합니다.

개발 용도의 경우 다음과 같은 기본 더미 공개 키가 MobileFirst Server와 함께 제공됩니다.

```xml
-----BEGIN PUBLIC KEY-----
MIIDPjCCAiagAwIBAgIEUD3/bjANBgkqhkiG9w0BAQsFADBgMQswCQYDVQQGEwJJTDELMAkGA1UECBMCSUwxETA
PBgNVBAcTCFNoZWZheWltMQwwCgYDVQQKEwNJQk0xEjAQBgNVBAsTCVdvcmtsaWdodDEPMA0GA1UEAxMGV0wgRG
V2MCAXDTEyMDgyOTExMzkyNloYDzQ3NTAwNzI3MTEzOTI2WjBgMQswCQYDVQQGEwJJTDELMAkGA1UECBMCSUwxE
TAPBgNVBAcTCFNoZWZheWltMQwwCgYDVQQKEwNJQk0xEjAQBgNVBAsTCVdvcmtsaWdodDEPMA0GA1UEAxMGV0wg
RGV2MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAzQN3vEB2/of7KAvuvyoIt0T7cjaSTjnOBm0N3+q
zx++dh92KpNJXj/a3o4YbwJXkJ7jU8ykjCYvjXRf0hme+HGhiIVwxJo54iqh76skDS5m7DaseFdndZUJ4p7NFVw
I5ixA36ZArSZ/Pn/ej56/RRjBeRI7AEGXUSGojBUPA6J6DYkwaXQRew9l+Q1kj4dTigyKL5Os0vNFaQyYu+bT2E
vnOixQ0DXm94IqmHZamZKbZLrWcOEfuAsSjKYOdMSM9jkCiHaKcj7fpEZhUxRRs7joKs1Ri4ihs6JeUvMEiG4gK
l9V3FP/Huy0pfkL0F8xMHgaQ4c/lxS/s3PV0OEg+7wIDAQABMA0GCSqGSIb3DQEBCwUAA4IBAQAgEhhqRl2Rgkt
MJeqOCRcT3uyr4XDK3hmuhEaE0nOvLHi61PoLKnDUNryWUicK/W+tUP9jkN5xRckdzG6TJ/HPySmZ7Adr6QRFu+
xcIMY+/S8j4PHLXBjoqgtUMhkt7S2/thN/VA6mwZpw4Ol0Pa2hyT2TkhQoYYkRwYCk9pxmuBCoH/eCWpSxquNny
RwrY25x0YzccXUaMI8L3/3hzq3mW40YIMiEdpiD5HqjUDpzN1funHNQdsxEIMYsWmGAwOdV5slFzyrH+ErUYUFA
pdGIdLtkrhzbqHFwXE0v3dt+lnLf21wRPIqYHaEu+EB/A4dLO6hm+IjBeu/No7H7TBFm
-----END PUBLIC KEY-----
```
{: codeblock}

**중요:** 프로덕션용으로는 공개 키를 사용하지 마십시오.

### 키 저장소 생성 및 배치
{: #generating-and-deploying-the-keystore }
인증서를 생성하고 키 저장소에서 공개 키를 추출하기 위해 여러 가지 도구를 사용할 수 있습니다. 다음 예에서는 JDK keytool 유틸리티 및 openSSL을 사용하는 프로시저를 설명합니다.

1. {{ site.data.keys.mf_server }}에 배치된 키 저장소 파일에서 공개 키를 추출하십시오.  
   >**참고:** 공개 키는 Base64로 인코딩되어야 합니다.

   예를 들어, 별명이 `mfp-server`이고 키 저장소 파일이 **keystore.jks**라고 가정합니다.  
   인증서를 생성하려면 다음 명령을 실행하십시오.

   ```bash
   keytool -export -alias mfp-server -file certfile.cert
   -keystore keystore.jks -storepass keypassword
   ```
   {: codeblock}

   인증서 파일이 생성됩니다.  
   다음 명령을 실행하여 공개 키를 추출하십시오.

   ```bash
   openssl x509 -inform der -in certfile.cert -pubkey -noout
   ```
   {: codeblock}

   > **참고:** keytool만으로 Base64 형식의 공개 키를 추출할 수는 없습니다.

2. 다음 프로시저 중 하나를 수행하십시오.
    * `BEGIN PUBLIC KEY` 및 `END PUBLIC KEY` 마커를 제외한 결과 텍스트를 애플리케이션의 mfpclient 특성 파일에서 `wlSecureDirectUpdatePublicKey` 바로 뒤에 복사하십시오.
    * 명령 프롬프트에서 다음 명령을 실행하십시오. `mfpdev app config direct_update_authenticity_public_key <public_key>`

    `<public_key>`의 경우 1단계에서 생성된 텍스트를 `BEGIN PUBLIC KEY` 및 `END PUBLIC KEY` 마커 없이 붙여넣으십시오.

3. cordova build 명령을 실행하여 공개 키를 애플리케이션에 저장하십시오.
