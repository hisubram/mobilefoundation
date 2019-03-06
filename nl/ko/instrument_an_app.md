---

copyright:
  years: 2018, 2019
lastupdated: "2019-01-30"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
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
{:web: .ph data-hd-programlang='Web'}

# 앱 인스트루먼트
{: #instrument_your_app}

Mobile Analytics는 {{ site.data.keyword.mobilefoundation_short }} 서비스에 임베드된 기능입니다. Mobile Analytics에서는 모바일 애플리케이션 개발자와 애플리케이션 소유자를 위한 주요 애플리케이션 사용 및 애플리케이션 성능 인사이트를 제공합니다.

Mobile Analytics 기능을 사용하여 애플리케이션 사용 및 성능을 모니터링하고 다른 통계를 얻기 시작하려면 모바일 애플리케이션을 인스트루먼트해야 합니다. 애플리케이션을 인스트루먼테이션하는 단계는 다음과 같습니다. 

1.  Mobile Analytics Client SDK를 가져와서 설치합니다.
2.  수집하려는 분석 데이터의 유형을 기반으로 애플리케이션을 인스트루먼트합니다.

다음 절에서는 지원되는 플랫폼별로 해당 단계를 자세히 설명합니다.

### Android 애플리케이션을 인스트루먼트합니다.
{: #instrument_android_app}
{: android}
#### 1단계: Mobile Analytics Client SDK for Android 가져오기 및 설치
{: #install_analytics_sdk_android }
{: android}
[![Maven Central](https://maven-badges.herokuapp.com/maven-central/com.ibm.mobile.foundation/ibmmobilefirstplatformfoundation/badge.svg)](https://maven-badges.herokuapp.com/maven-central/com.ibm.mobile.foundation/ibmmobilefirstplatformfoundation)
[![Maven Central](https://maven-badges.herokuapp.com/maven-central/com.ibm.mobile.foundation/ibmmobilefirstplatformfoundationanalytics/badge.svg)](https://maven-badges.herokuapp.com/maven-central/com.ibm.mobile.foundation/ibmmobilefirstplatformfoundationanalytics)
{: android}

Mobile Analytics Client SDK는 Android 프로젝트의 종속 관리자인 Gradle을 사용하여 배포됩니다. Gradle은 저장소에서 아티팩트를 자동으로 다운로드하여 Android 애플리케이션에 제공합니다.
{: android}

1. [Android Studio ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://developer.android.com/sdk/index.html){: new_window} 프로젝트를 작성하거나 기존 프로젝트를 여십시오.
{: android}

2. **앱 모듈**에 있는 `build.gradle` 파일을 여십시오.
{: android}

  Android 프로젝트에는 각각 프로젝트와 앱 모듈용으로 한 개씩 두 개의 `build.gradle` 파일이 있을 수 있습니다. **앱 모듈** 파일을 사용해야 합니다.
  {: tip}
  {: android}

3. `build.gradle` 파일의 `Dependencies` 섹션을 찾아 {{site.data.keyword.mobileanalytics_short}} 클라이언트 SDK의 컴파일 종속성을 추가하십시오. 저장소 명령문은 다음 코드 예와 유사해야 합니다.
{: android}

  ```
  dependencies {
    compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundation:8.0.+'
    compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundationanalytics:8.0.+@aar'
    // other dependencies
  }
  ```
  {: codeblock}
  {: android}

  첫 번째 종속성은 애플리케이션 런타임 이벤트를 캡처하고 로깅하는 데 사용하는 Mobile Analytics Client SDK용이고
  두 번째 종속성은 인앱(in-app) 사용자 피드백을 사용으로 설정하여 애플리케이션 사용자와 상호작용하는 데 사용합니다. 인앱 사용자 피드백을 사용하는 경우에만 두 번째 종속성이 필요합니다. 
  {: android}

4. **도구 &gt; Android &gt; 프로젝트와 Gradle 파일 동기화**를 클릭하여 프로젝트와 Gradle을 동기화하십시오.
{: android}

5. Android 프로젝트의 `AndroidManifest.xml` 파일을 여십시오. 이 파일은 **앱 > Manifest**에 있습니다. 다음과 같이 `<manifest>` 요소에 인터넷 액세스와 위치 액세스 권한을 추가하십시오.
{: android}

  ```xml
   <uses-permission android:name="android.permission.INTERNET" />
 
  ```
  {: codeblock}
  1.2 이상의 SDK 버전을 사용하는 경우 `AndroidManifest.xml` 파일의 `<application>` 요소 안에 다음 행을 두어야 합니다.
  {: android}

  ```
    <activity
  android:name="com.ibm.mobilefirstplatform.clientsdk.android.ui.UIActivity"
  android:label="@string/app_name"
  android:launchMode="singleTask">
  <intent-filter>
  <action android:name="android.intent.action.MAIN" />
  <category android:name="android.intent.category.LAUNCHER" />
  </intent-filter>
  </activity>
  ```
  {: codeblock}
  {: android}

#### 2단계: 수집하려는 분석 데이터의 유형을 기반으로 Android 애플리케이션 인스트루먼트
{: #instrument_app_based_on_data_android }
{: android}

1. 분석 데이터를 캡처하여 Mobile Analytics 서비스에 보내기 위해 애플리케이션을 초기화합니다.  먼저, 다음 `import`문을 애플리케이션 또는 활동 클래스의 시작 부분에 추가하십시오.
{: android}

   ```Java
    import com.worklight.common.Logger;
    import com.worklight.common.WLAnalytics;
   ```
   {: codeblock}
   {: android}

   다음으로 Mobile Analytics Client SDK를 사용하여 분석 데이터의 캡처를 설정하거나 초기화하십시오.  
   애플리케이션 프로젝트에 가장 적합한 위치나 Android 애플리케이션에 있는 기본 활동의 `onCreate` 메소드에 초기화 코드를 추가하십시오.
   {: android}

   ```Java
    WLAnalytics.init(this.getApplication());
   ```
   {: codeblock}
   {: android}

   init 메소드를 호출하기 전에 애플리케이션에서 MobileFoundation 서비스를 사용하여 디바이스를 인증하고 권한을 부여하는 데 필요한 코드를 임베드하는지 확인해야 합니다.  이 단계는 모든 Mobile Foundation 서비스 애플리케이션에 필수이며 분석 데이터 캡처에 고유하지 않은 공통 단계입니다. <!--  Refer <need to link doc that talks about auth> -->
   {: android}

   초기화가 완료되면 이제 추가 코드를 추가하지 않아도 애플리케이션에서 디바이스 정보 및 Mobile Analytics SDK 로그를 캡처할 수 있습니다.  다음 섹션에서 설명한 추가 API 및 코드는 선택적이며 캡처하려는 분석 데이터 종류에 따라 추가할 수 있습니다.
   {: android}

2. 애플리케이션 세션 및 충돌 정보와 같은 애플리케이션 라이프사이클 이벤트를 캡처하려면 다음을 추가하여 애플리케이션을 구성하십시오.
  ```Java
    WLAnalytics.addDeviceEventListener(WLAnalytics.DeviceEvent.LIFECYCLE);
  ```
  {: codeblock}
  {: android}

3. 애플리케이션 네트워크 상호작용을 캡처하는 네트워크 이벤트를 캡처하려면 다음을 추가하여 애플리케이션을 구성하십시오. 
  ```Java
    WLAnalytics.addDeviceEventListener(WLAnalytics.DeviceEvent.NETWORK);
  ```
  {: codeblock}
  {: android}

4. 이제 분석 데이터를 캡처하도록 애플리케이션이 초기화되었습니다.  다음으로, 캡처된 데이터를 Mobile Analytics 서비스로 전송해야 합니다.
   다음 API를 사용하여 Mobile Analytics 서비스에 분석 데이터를 전송하십시오.
   ```java
    WLAnalytics.send();
   ```
   {: codeblock}
   {: android}

  캡처된 분석 데이터를 Mobile Analytics 서비스로 전송하기 위해 애플리케이션 플로우에서 이 API를 호출할 시기를 자유롭게 결정할 수 있습니다.  캡처된 모든 분석 데이터는 전송될 때까지 디바이스에 로컬로 저장됩니다.
  {: android}

5. 애플리케이션 로그를 캡처하여 Mobile Analytics 서비스에 전송하려면 Mobile Analytics Client SDK 로거 API를 사용하십시오.     
   Mobile Analytics Client SDK 로깅 프레임워크에서는 다음과 같은 로그 레벨을 지원합니다. 이 레벨은 가장 간단한 로깅에서 가장 상세한 로깅 순으로 나열되어 있으며 권장 사용 가이드라인이 함께 제공됩니다.
    * FATAL - 복구할 수 없는 충돌 또는 정지가 발생한 경우 사용합니다. FATAL 레벨은 애플리케이션이 충돌할 때 사용자에게 표시되는 복구할 수 없는 오류를 로깅하는 데 사용하도록 예약되어 있습니다.
    * ERROR - 예상치 못한 예외 또는 예상치 못한 네트워크 프로토콜 오류에 사용합니다.
    * WARN - 더 이상 사용되지 않는 API나 느린 네트워크 응답과 같이 중요한 오류로 간주되지 않는 사용 경고를 로깅하는 데 사용합니다.
    * INFO - 중요하지만 긴급하지는 않은 초기화 이벤트 및 기타 데이터를 보고하는 데 사용합니다.
    * DEBUG - 개발자가 애플리케이션 결함을 해결하는 데 도움이 되도록 디버그 명령문을 보고하는 데 사용합니다.
    로거 레벨이 DEBUG로 설정된 경우에는 Mobile Analytics 클라이언트 SDK 로그도 제공됩니다. 이 로그는 로그를 전송할 때 포함됩니다.
   {: note}
   {: android}

   다음 코드 스니펫에서는 Android의 샘플 로거 사용법을 보여줍니다.
   ```java
   // Set the logging level (optional)
   // The default setting is Logger.LEVEL.DEBUG
   Logger.setLogLevel(Logger.LEVEL.INFO);
    
   //Create a logger instance.  You can create multiple loggers to organize your logs as you wish
   Logger logger = Logger.getInstance("loggerName");
   
   // Log messages with different levels
   // Debug message for feature 1
   // Info message for feature 2
   logger.debug("debug message");
   //the logger.debug message is not logged because the logLevelFilter is set to Info
   logger.info("info message");
      
   // Send logs to the Mobile Analytics
   Logger.send();
   ```
   {: codeblock}
   {: android}

6.  사용자 참여 패턴(새로운 사용자 대 돌아온 사용자)을 이해하려면 사용자 ID를 애플리케이션 세션과 연관시켜야 합니다.  이 작업은 다음 API를 호출하여 수행할 수 있습니다.
    ```java
      WLAnalytics.setUserContext("userName or userIdentity");
    ```
    {: codeblock}
    {: android}

    로컬에서 캡처되고 저장된 모든 분석 데이터는 WLAnalytics.send() API가 호출될 때만 Mobile Analytics 서비스로 전송됩니다.
    {: android}

7.  호출된 HTTP API, 요청 수 및 평균 응답 시간과 같은 애플리케이션 HTTP 네트워크 상호작용 패턴을 이해하려면 Mobile Foundation Service Client SDK의 WLResourceRequest 클래스를 사용하여 HTTP 호출을 수행해야 합니다.  네트워크 이벤트를 캡처하기 위해 Analytics를 초기화한 경우에도 WLResourceRequest를 사용하면 Mobile Anayltics가 네트워크 호출에 후크하여 관련 데이터를 캡처할 수 있습니다.  다음과 같은 방법으로 HTTP 호출을 수행해야 합니다.
    ```java
      WLResourceRequest request = new WLResourceRequest(new URI(url), WLResourceRequest.GET);
            request.send(new WLResponseListener() {

                @Override
                    public void onSuccess(WLResponse wlResponse) {
                    //handle success of HTTP call and use wlResponse 
                }

                @Override
            public void onFailure(WLFailResponse wlFailResponse) {
                    //handle failure of HTTP call and use wlFailResponse
                }
            });
    ```
    {: codeblock}
    {: android}

8.  충돌 분석 및 로그를 가져오려면 애플리케이션 시작 중에 다음 API를 호출하여 이전 세션이 충돌되었는지 확인하고 캡처 충돌 로그를 적절하게 Mobile Analytics 서비스에 보낼 수 있습니다.
    ```java
      if (Logger.isUnCaughtExceptionDetected()) {
            //add any other handling you may want and call the following APIs
            WLAnalytics.send();
            Logger.send();
      }
    ```
    {: codeblock}
    {: android}

9.  Client SDK에서 기본적으로 지원하는 수준 이상으로 사용자 정의 분석을 정의하고 고유 분석 데이터를 정의하려면 사용자 정의 로깅 API를 사용할 수 있습니다.
    ```java
        //create a JSON to capture the custom data 
        JSONObject jsonObject = new JSONObject();
        jsonObject.put("FromPage", "LoginPage");
   
        //log the custom data with a message
        WLAnalytics.log("log message", jsonObject);
        
        //Send the captured custom data and log to the Mobile Analytics service
        WLAnalytics.send();
    ```
    {: codeblock}
    {: android}

    로깅된 사용자 정의 데이터를 Mobile Analytics 콘솔에서 정의할 수 있는 사용자 정의 차트에 표시하여 사용자 정의 인사이트를 도출할 수 있습니다.
    {: android}

10. 인앱 사용자 피드백을 사용하여 애플리케이션 성능 분석을 강화하십시오.   애플리케이션 **사용자 및 테스터**를 사용하여 앱 소유자에게 풍부한 컨텍스트별 피드백을 제공할 수 있습니다. **앱 소유자**가 사용자로부터 애플리케이션 사용 경험에 관한 실시간 피드백을 받으므로 **앱 소유자**와 **개발자**가 해당 경험에 관해 추가로 조치를 취할 수 있습니다.  그러면 상당히 민첩하게 애플리케이션을 유지할 수 있습니다.  예를 들어, 단추 클릭 또는 메뉴 항목 선택을 처리할 때 애플리케이션의 조치 핸들러에서 대화식 피드백 모드로 애플리케이션을 전환하려면 다음 API를 사용하십시오.
    ```java
        WLAnalytics.triggerFeedbackMode();
    ```
    {: codeblock}
    {: android}

### iOS 애플리케이션 인스트루먼트
{: #instrument_ios_app}
{: ios}

iOS 애플리케이션을 인스트루먼트하는 단계는 다음과 같습니다.
{: ios}

#### 1단계: Mobile Analytics Client SDK for iOS 가져오기 및 설치
{: #install_analytics_sdk_ios }
{: ios}

[![CocoaPods 호환 가능](https://img.shields.io/cocoapods/v/IBMMobileFirstPlatformFoundation.svg)](https://cocoapods.org/pods/IBMMobileFirstPlatformFoundation)
[![CocoaPods 호환 가능](https://img.shields.io/cocoapods/v/IBMMobileFirstPlatformFoundationAnalytics.svg)](https://cocoapods.org/pods/IBMMobileFirstPlatformFoundationAnalytics)
{: ios}

Swift SDK는 iOS 및 watchOS에 사용할 수 있습니다.
{: ios}

1. Xcode 프로젝트의 루트에 있는 기존 podfile에 다음을 추가하십시오.
   ```bash
   use_frameworks!
   platform :ios, 8.0
   target "ProjectName" do
       pod 'IBMMobileFirstPlatformFoundation'
       pod 'IBMMobileFirstPlatformFoundationAnalytics'
   end
   ```
   {: codeblock}
   {: ios}

2. 명령행 창에서 Xcode 프로젝트의 루트로 이동하여 다음 명령을 실행하십시오.
   ```bash
   pod update
   ```
   {: codeblock}
   {: ios}

#### 2단계: 수집하려는 분석 데이터의 유형을 기반으로 iOS 애플리케이션 인스트루먼트
{: #instrument_app_based_on_data_ios }
{: ios}

1. Mobile Analytics에 로그를 전송할 수 있도록 애플리케이션을 초기화하십시오.
   Swift SDK는 iOS 및 watchOS에 사용할 수 있습니다. 다음 `import` 문을 `AppDelegate.swift` 프로젝트 파일의 시작 부분에 추가하여 `BMSCore` 프레임워크와 `BMSAnalytics` 프레임워크를 가져오십시오.
    ```Swift
    import IBMMobileFirstPlatformFoundation
    ```
    {: codeblock}
    {: ios}

   다음으로 Mobile Analytics Client SDK를 사용하여 분석 데이터의 캡처를 설정하거나 초기화하십시오.
   애플리케이션 프로젝트에 가장 적합한 위치에 초기화 코드를 추가하십시오.
   {: ios}

   ```Swift
    WLAnalytics.init()
   ```
   {: codeblock}
   {: ios}

   init 메소드를 호출하기 전에 애플리케이션에서 MobileFoundation 서비스를 사용하여 디바이스를 인증하고 권한을 부여하는 데 필요한 코드를 임베드하는지 확인해야 합니다.  이 단계는 모든 Mobile Foundation 서비스 애플리케이션에 필수이며 분석 데이터 캡처에 고유하지 않은 공통 단계입니다. <!--  Refer <need to link doc that talks about auth> -->
   {: ios}

   초기화가 완료되면 이제 추가 코드를 추가하지 않아도 애플리케이션에서 디바이스 정보 및 Mobile Analytics SDK 로그를 캡처할 수 있습니다.  다음 섹션에서 설명한 추가 API 및 코드는 선택적이며 캡처하려는 분석 데이터 종류에 따라 추가할 수 있습니다.
   {: ios}

2. 애플리케이션 세션 및 충돌 정보와 같은 애플리케이션 라이프사이클 이벤트를 캡처하려면 다음을 추가하여 애플리케이션을 구성하십시오.
    ```Swift
      WLAnalytics.sharedInstance().addDeviceEventListener(LIFECYCLE)
    ```
    {: codeblock}
    {: ios}

3. 애플리케이션 네트워크 상호작용을 캡처하는 네트워크 이벤트를 캡처하려면 다음을 추가하여 애플리케이션을 구성하십시오.
    ```Swift
      WLAnalytics.sharedInstance().addDeviceEventListener(NETWORK)
    ```
    {: codeblock}
    {: ios}

4. 이제 분석 데이터를 캡처하도록 애플리케이션이 초기화되었습니다.  다음으로, 캡처된 데이터를 Mobile Analytics 서비스로 전송해야 합니다.
   다음 API를 사용하여 Mobile Analytics 서비스에 분석 데이터를 전송하십시오.
   ```Swift
    WLAnalytics.sharedInstance().send();
   ```
   {: codeblock}
   {: ios}

    캡처된 분석 데이터를 Mobile Analytics 서비스로 전송하기 위해 애플리케이션 플로우에서 이 API를 호출할 시기를 자유롭게 결정할 수 있습니다.  캡처된 모든 분석 데이터는 전송될 때까지 디바이스에 로컬로 저장됩니다.
    {: ios}

5. 애플리케이션 로그를 캡처하여 Mobile Analytics 서비스에 전송하려면 Mobile Analytics Client SDK 로거 API를 사용하십시오.
      Mobile Analytics Client SDK 로깅 프레임워크에서는 다음과 같은 로그 레벨을 지원합니다. 이 레벨은 가장 간단한 로깅에서 가장 상세한 로깅 순으로 나열되어 있으며 권장 사용 가이드라인이 함께 제공됩니다.
    * FATAL - 복구할 수 없는 충돌 또는 정지가 발생한 경우 사용합니다. FATAL 레벨은 애플리케이션이 충돌할 때 사용자에게 표시되는 복구할 수 없는 오류를 로깅하는 데 사용하도록 예약되어 있습니다.
    * ERROR - 예상치 못한 예외 또는 예상치 못한 네트워크 프로토콜 오류에 사용합니다.
    * WARN - 더 이상 사용되지 않는 API나 느린 네트워크 응답과 같이 중요한 오류로 간주되지 않는 사용 경고를 로깅하는 데 사용합니다.
    * INFO - 중요하지만 긴급하지는 않은 초기화 이벤트 및 기타 데이터를 보고하는 데 사용합니다.
    * DEBUG - 개발자가 애플리케이션 결함을 해결하는 데 도움이 되도록 디버그 명령문을 보고하는 데 사용합니다.
    로거 레벨이 DEBUG로 설정된 경우에는 Mobile Analytics 클라이언트 SDK 로그도 제공됩니다. 이 로그는 로그를 전송할 때 포함됩니다.
   {: note}
    {: ios}

    iOS 애플리케이션에 로깅 기능을 추가하기 위해 로거의 코드 스니펫을 보려면 [튜토리얼 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://mobilefirstplatform.ibmcloud.com/tutorials/it/foundation/8.0/application-development/client-side-log-collection/ios/)을 따르십시오.

6.  사용자 참여 패턴(새로운 사용자 대 돌아온 사용자)을 이해하려면 사용자 ID를 애플리케이션 세션과 연관시켜야 합니다.  이 작업은 다음 API를 호출하여 수행할 수 있습니다.
    ```Swift
      WLAnalytics.sharedInstance().setUserContext("userName or userIdentity")
    ```
    {: codeblock}
    {: ios}

    로컬에서 캡처되고 저장된 모든 분석 데이터는 WLAnalytics.sharedInstance().send() API가 호출될 때만 Mobile Analytics 서비스로 전송됩니다.
    {: ios}

7.  호출된 HTTP API, 요청 수 및 평균 응답 시간과 같은 애플리케이션 HTTP 네트워크 상호작용 패턴을 이해하려면 Mobile Foundation Service Client SDK의 WLResourceRequest 클래스를 사용하여 HTTP 호출을 수행해야 합니다.  네트워크 이벤트를 캡처하기 위해 Analytics를 초기화한 경우에도 WLResourceRequest를 사용하면 Mobile Anayltics가 네트워크 호출에 후크하여 관련 데이터를 캡처할 수 있습니다.  다음과 같은 방법으로 HTTP 호출을 수행해야 합니다.
    ```Swift
      let request = WLResourceRequest( url: URL(string: "/adapters/JavaAdapter/users"), method: WLHttpMethodGet )

      request!.send { (response, error) -> Void in
          if(error == nil){
              //handle failure of HTTP call and use wlFailResponse
          }
          else{
              //handle success of HTTP call and use wlResponse
          }
      }
    ```
    {: codeblock}
    {: ios}

8.  충돌 분석 및 로그를 가져오려면 애플리케이션 시작 중에 다음 API를 호출하여 이전 세션이 충돌되었는지 확인하고 캡처 충돌 로그를 적절하게 Mobile Analytics 서비스에 보낼 수 있습니다.
    ```Swift
      if (OCLogger.isUnCaughtExceptionDetected()) {
            //add any other handling you may want and call the following APIs
            WLAnalytics.sharedInstance().send();
            OCLogger.send();
      }
    ```
    {: codeblock}
    {: ios}

9.  Client SDK에서 기본적으로 지원하는 수준 이상으로 사용자 정의 분석을 정의하고 고유 분석 데이터를 정의하려면 사용자 정의 로깅 API를 사용할 수 있습니다.
    ```Swift
        //create an array object with key value pair as custom data
        let eventObject = ["FromPage": "LoginPage"]

        //log the custom data with a message
        WLAnalytics.sharedInstance().log("log message", withMetadata: eventObject);

        //Send the captured custom data and log to the Mobile Analytics service
        WLAnalytics.sharedInstance().send();
    ```
    {: codeblock}
    {: ios}

    로깅된 사용자 정의 데이터를 Mobile Analytics 콘솔에서 정의할 수 있는 사용자 정의 차트에 표시하여 사용자 정의 인사이트를 도출할 수 있습니다.
    {: ios}

10. 인앱 사용자 피드백을 사용하여 애플리케이션 성능 분석을 강화하십시오.   애플리케이션 **사용자 및 테스터**를 사용하여 앱 소유자에게 풍부한 컨텍스트별 피드백을 제공할 수 있습니다. **앱 소유자**가 사용자로부터 애플리케이션 사용 경험에 관한 실시간 피드백을 받으므로 **앱 소유자**와 **개발자**가 해당 경험에 관해 추가로 조치를 취할 수 있습니다.  그러면 상당히 민첩하게 애플리케이션을 유지할 수 있습니다.  예를 들어, 단추 클릭 또는 메뉴 항목 선택을 처리할 때 애플리케이션의 조치 핸들러에서 대화식 피드백 모드로 애플리케이션을 전환하려면 다음 API를 사용하십시오.
    ```Swift
        WLAnalytics.sharedInstance().triggerFeedbackMode();
    ```
    {: codeblock}
    {: ios}

### Cordova 애플리케이션 인스트루먼트
{: #instrument_cordova_app}
{: cordova}
Cordova 애플리케이션을 인스트루먼트하는 단계는 다음과 같습니다.
{: cordova}

#### 1단계: Foundation and Analytics Client SDK plugin for Cordova 가져오기 및 설치
{: #install_analytics_sdk_cordova }
Mobile Analytics Cordova 플러그인을 사용하면 모바일 애플리케이션을 인스트루먼트할 수 있습니다.
{: cordova}

#### Cordova Client SDK 설치
{: #install_cordova_sdk}
{: cordova}

1. [Cordova ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://cordova.apache.org/#getstarted){: new_window} 프로젝트를 작성하거나 기존 프로젝트를 여십시오.
    {: cordova}

2. 선택한 Android 또는 iOS 플랫폼을 Cordova 애플리케이션에 추가하십시오. 명령행에서 다음 명령 중 하나 또는 둘 다를 실행하십시오.<br/>
    **Android**:
    ```
    cordova platform add android
    ```
    {: codeblock}
    {: cordova}

    **iOS**:
    ```
    cordova platform add ios
    ```
    {: codeblock}
    {: cordova}

3. Foundation Client SDK 플러그인 `cordova-plugin-mfp`를 추가하십시오.
   ```bash
   cordova plugin add cordova-plugin-mfp
   ```
   {: codeblock}
   {: cordova}
4. 인앱(in-app) 피드백 기능인 선택적 Analytics Client SDK 플러그인 `cordova-plugin-mfp-analytics`를 앱에 추가하십시오.
    {: cordova}
    ```bash
    cordova plugin add cordova-plugin-mfp-analytics
    ```
    {: codeblock}
    {: cordova}
5. 다음 명령을 실행하여 플러그인이 설치되었는지 확인하십시오.
    {: cordova}
    ```bash
    cordova plugin list
    ```
    {: codeblock}
    {: cordova}

#### 2단계: 수집하려는 분석 데이터의 유형을 기반으로 Cordova 애플리케이션 인스트루먼트
{: #instrument_app_based_on_data_cordova }
{: cordova}

1. Cordova 애플리케이션에서는 설정이 필요하지 않고 초기화가 기본 제공됩니다. 

   아래 분석 메소드를 호출하기 전에 애플리케이션에서 MobileFoundation 서비스를 사용하여 디바이스를 인증하고 권한을 부여하는 데 필요한 코드를 임베드하는지 확인해야 합니다.  이 단계는 모든 Mobile Foundation 서비스 애플리케이션에 필수이며 분석 데이터 캡처에 고유하지 않은 공통 단계입니다. <!--  Refer <need to link doc that talks about auth> -->
   {: cordova}

   초기화가 완료되면 이제 추가 코드를 추가하지 않아도 애플리케이션에서 디바이스 정보 및 Mobile Analytics SDK 로그를 캡처할 수 있습니다.  다음 섹션에서 설명한 추가 API 및 코드는 선택적이며 캡처하려는 분석 데이터 종류에 따라 추가할 수 있습니다.
   {: cordova}

2. 라이프사이클 이벤트의 캡처를 사용하려면 Cordova 앱의 기본 플랫폼에서 초기화해야 합니다.
    * Android 플랫폼의 경우:
      * **[Cordova 애플리케이션 루트 폴더] → platforms → android → src → com → sample → [app-name] → MainActivity.java** 파일을 여십시오.
      * `onCreate` 메소드를 찾고 아래 단계에 따라 `LIFECYCLE` 및 `NETWORK` 활동을 사용으로 설정하십시오.
        * 클라이언트 라이프사이클 이벤트 로깅을 사용하려면 다음을 수행하십시오.
          ```Java
          WLAnalytics.addDeviceEventListener(DeviceEvent.LIFECYCLE);
          ```
          {: codeblock}
          {: cordova}
        * 클라이언트 네트워크-이벤트 로깅을 사용하려면 다음을 수행하십시오.
          ```Java
          WLAnalytics.addDeviceEventListener(DeviceEvent.NETWORK);
          ```
          {: codeblock}
          {: cordova}
      * `cordova build` 명령을 실행하여 Cordova 프로젝트를 빌드하십시오.
    * iOS 플랫폼의 경우:
      * **[Cordova 애플리케이션 루트 폴더] → platforms → ios → Classes** 폴더를 열고 **AppDelegate.swift**(Swift) 파일을 찾으십시오.
      * 아래 단계를 따라 `LIFECYCLE` 및 `NETWORK` 활동을 사용으로 설정하십시오.
        * 클라이언트 라이프사이클 이벤트 로깅을 사용하려면 다음을 수행하십시오.
          ```Swift
          WLAnalytics.sharedInstance().addDeviceEventListener(LIFECYCLE);
          ```
          {: codeblock}
          {: cordova}
        * 클라이언트 네트워크-이벤트 로깅을 사용하려면 다음을 수행하십시오.
          ```Swift
          WLAnalytics.sharedInstance().addDeviceEventListener(NETWORK);
          ```
          {: codeblock}
          {: cordova}
      * `cordova build` 명령을 실행하여 Cordova 프로젝트를 빌드하십시오.

3. 이제 분석 데이터를 수집하도록 애플리케이션이 초기화되었습니다. 다음으로, 분석 데이터를 Mobile Analytics에 전송할 수 있습니다.
   다음 API를 사용하여 사용 분석 기록 및 전송을 시작하십시오.
   ```Javascript
   // Send recorded usage analytics to the Mobile Analytics

   WL.Analytics.send();
   ```
   {: codeblock}
   {: cordova}

4. 애플리케이션 로그를 캡처하여 Mobile Analytics 서비스에 전송하려면 Mobile Analytics Client SDK 로거 API를 사용하십시오.
   Mobile Analytics Client SDK 로깅 프레임워크에서는 다음과 같은 로그 레벨을 지원합니다. 이 레벨은 가장 간단한 로깅에서 가장 상세한 로깅 순으로 나열되어 있으며 권장 사용 가이드라인이 함께 제공됩니다.
    * FATAL - 복구할 수 없는 충돌 또는 정지가 발생한 경우 사용합니다. FATAL 레벨은 애플리케이션이 충돌할 때 사용자에게 표시되는 복구할 수 없는 오류를 로깅하는 데 사용하도록 예약되어 있습니다.
    * ERROR - 예상치 못한 예외 또는 예상치 못한 네트워크 프로토콜 오류에 사용합니다.
    * WARN - 더 이상 사용되지 않는 API나 느린 네트워크 응답과 같이 중요한 오류로 간주되지 않는 사용 경고를 로깅하는 데 사용합니다.
    * INFO - 중요하지만 긴급하지는 않은 초기화 이벤트 및 기타 데이터를 보고하는 데 사용합니다.
    * DEBUG - 개발자가 애플리케이션 결함을 해결하는 데 도움이 되도록 디버그 명령문을 보고하는 데 사용합니다.
    로거 레벨이 DEBUG로 설정된 경우에는 Mobile Analytics 클라이언트 SDK 로그도 제공됩니다. 이 로그는 로그를 전송할 때 포함됩니다.
    * TRACE - 메소드 시작점 및 종료점에 사용됩니다.
   {: note}
   {: cordova}

   다음 코드 스니펫에서는 Cordova의 샘플 로거 사용법을 보여줍니다.
    ```Javascript
    // Set the logging level (optional)
    // The default setting is DEBUG
    WL.Logger.config({"level": "INFO"});

    //Create a logger instance.  You can create multiple loggers to organize your logs as you wish
    var logger = WL.Logger.create({ pkg: 'loggerName' });

    // Log messages with different levels
    // Debug message for feature 1
    // Info message for feature 2
    logger.debug("debug","debug message");
    //the logger.debug message is not logged because the logLevelFilter is set to Info
    logger.info("info","info message");

    // Send logs to the Mobile Analytics
    logger.send();
    ```
    {: codeblock}
    {: cordova}

5.  사용자 참여 패턴(새로운 사용자 대 돌아온 사용자)을 이해하려면 사용자 ID를 애플리케이션 세션과 연관시켜야 합니다.  Javascript 레벨에서 사용할 수 있는 특정 API가 없습니다. 그러나 이 작업은 다음 API를 호출하여 네이티브 코드에서 수행할 수 있습니다.

    **Android:**
      ```java
        WLAnalytics.setUserContext("userName or userIdentity");
      ```
      {: codeblock}
      {: cordova}

    **iOS:**
      ```Swift
        WLAnalytics.sharedInstance().setUserContext("userName or userIdentity")
      ```
      {: codeblock}
      {: cordova}

    로컬에서 캡처되고 저장된 모든 분석 데이터는 javascript 레벨의 WL.Analytics.send() API [또는] android 네이티브 WLAnalytics.send() API [또는] ios 네이티브 WLAnalytics.sharedInstance().send() API를 호출할 때만 Mobile Analytics 서비스에 전송됩니다.
    {: cordova}

6. 호출된 HTTP API, 요청 수 및 평균 응답 시간과 같은 애플리케이션 HTTP 네트워크 상호작용 패턴을 이해하려면 Mobile Foundation Service Client SDK의 WLResourceRequest 클래스를 사용하여 HTTP 호출을 수행해야 합니다.  네트워크 이벤트를 캡처하기 위해 Analytics를 초기화한 경우에도 WLResourceRequest를 사용하면 Mobile Anayltics가 네트워크 호출에 후크하여 관련 데이터를 캡처할 수 있습니다.  다음과 같은 방법으로 HTTP 호출을 수행해야 합니다.
    ```Javascript
      var resourceRequest = new WLResourceRequest("url-path", WLResourceRequest.GET);
      resourceRequest.send().then(
        onSuccess: function(response) {
            resultText = "Successfully called the resource: " + response.responseText;
        },

        onFailure: function(response) {
            resultText = "Failed to call the resource:" + response.errorMsg;
        }
      );
    ```
    {: codeblock}
    {: cordova}

7. Client SDK에서 기본적으로 지원하는 수준 이상으로 사용자 정의 분석을 정의하고 고유 분석 데이터를 정의하려면 사용자 정의 로깅 API를 사용할 수 있습니다.
    ```Javascript
        //create custom data as key value pair
        WL.Analytics.log({"FromPage" : 'LoginPage'});

        //Send the captured custom data and log to the Mobile Analytics service
        WL.Analytics.send();
    ```
    {: codeblock}
    {: cordova}

    로깅된 사용자 정의 데이터를 Mobile Analytics 콘솔에서 정의할 수 있는 사용자 정의 차트에 표시하여 사용자 정의 인사이트를 도출할 수 있습니다.
    {: cordova}

8. 인앱 사용자 피드백을 사용하여 애플리케이션 성능 분석을 강화하십시오.   애플리케이션 **사용자 및 테스터**를 사용하여 앱 소유자에게 풍부한 컨텍스트별 피드백을 제공할 수 있습니다. **앱 소유자**가 사용자로부터 애플리케이션 사용 경험에 관한 실시간 피드백을 받으므로 **앱 소유자**와 **개발자**가 해당 경험에 관해 추가로 조치를 취할 수 있습니다.  그러면 상당히 민첩하게 애플리케이션을 유지할 수 있습니다.  예를 들어, 단추 클릭 또는 메뉴 항목 선택을 처리할 때 애플리케이션의 조치 핸들러에서 대화식 피드백 모드로 애플리케이션을 전환하려면 다음 API를 사용하십시오.
    ```Javascript
        WL.Analytics.triggerFeedbackMode();
    ```
    {: codeblock}
    {: cordova}

### 웹 애플리케이션 인스트루먼트
{: #instrument_web_app}
{: web}

웹 애플리케이션을 인스트루먼트하는 단계는 다음과 같습니다.
{: web}

#### 1단계: Mobile Analytics Client SDK for Web 가져오기 및 설치
{: #install_analytics_sdk_web }
{: web}

#### Web Client SDK 설치
{: #install_web_sdk}
Mobile Analytics SDK를 사용하면 웹 애플리케이션을 인스트루먼트할 수 있습니다.
{: web}

1. 웹 애플리케이션의 루트 폴더로 이동하여 다음 명령을 실행하십시오. [npm](https://www.npmjs.com/package/ibm-mfp-web-sdk)을 사용하여 웹 애플리케이션에 SDK를 추가하십시오.
    ```bash
    npm install ibm-mfp-web-sdk
    ```
    {: codeblock}
    {: web}


#### 2단계: 수집하려는 분석 데이터의 유형을 기반으로 웹 애플리케이션 인스트루먼트
{: #instrument_app_based_on_data_web }
{: web}

1. 기본 html 페이지의 `head` 요소에 있는 ibmmfpf.js 및 ibmmfpfanalytics.js 파일을 참조

    ```html
      <head>
        ....
        <script type="text/javascript" src="node_modules/ibm-mfp-web-sdk/ibmmfpf.js"></script>
        <script type="text/javascript" src="node_modules/ibm-mfp-web-sdk/lib/analytics/ibmmfpfanalytics.js"></script>
      </head>
    ```
    {: codeblock}
    {: web}

2. 웹 애플리케이션의 기본 JavaScript 파일에서 컨텍스트 루트 및 애플리케이션 ID 값을 지정하여 Mobile Foundation Web SDK 초기화

    ```Javascript
      var wlInitOptions = {
        mfpContextRoot : '/mfp', // "mfp" is the default context root in the Mobile Foundation
        applicationId : 'com.sample.mywebapp' // Replace with your own value.
      };

      WL.Client.init(wlInitOptions).then (
        function() {
          // Application logic.
        }
      );
    ```
    {: codeblock}
    {: web}

   아래 분석 메소드를 호출하기 전에 애플리케이션에서 MobileFoundation 서비스를 사용하여 디바이스를 인증하고 권한을 부여하는 데 필요한 코드를 임베드하는지 확인해야 합니다.  이 단계는 모든 Mobile Foundation 서비스 애플리케이션에 필수이며 분석 데이터 캡처에 고유하지 않은 공통 단계입니다. <!--  Refer <need to link doc that talks about auth> -->
   {: web}

   초기화가 완료되면 이제 추가 코드를 추가하지 않아도 애플리케이션에서 디바이스 정보 및 Mobile Analytics SDK 로그를 캡처할 수 있습니다.  다음 섹션에서 설명한 추가 API 및 코드는 선택적이며 캡처하려는 분석 데이터 종류에 따라 추가할 수 있습니다.
   {: web}

3. 애플리케이션 라이프사이클 이벤트 및 네트워크 이벤트를 캡처하려면 애플리케이션 로직에 다음 행을 추가하십시오.

    ```Javascript
    ibmmfpfanalytics.logger.config({analyticsCapture: true});
    ```
    {: codeblock}
    {: web}

4. 이제 분석 데이터를 캡처하도록 애플리케이션이 초기화되었습니다.  다음으로 캡처된 데이터를 Mobile Analytics 서비스에 전송해야 합니다. 다음 API를 사용하여 Mobile Analytics 서비스에 분석 데이터를 전송하십시오.

   ```Javascript
   ibmmfpfanalytics.send();
   ```
   {: codeblock}
   {: web}

    캡처된 분석 데이터를 Mobile Analytics 서비스로 전송하기 위해 애플리케이션 플로우에서 이 API를 호출할 시기를 자유롭게 결정할 수 있습니다.  캡처된 모든 분석 데이터는 전송될 때까지 디바이스에 로컬로 저장됩니다.
    {: web}

5. 애플리케이션 로그를 캡처하여 Mobile Analytics 서비스에 전송하려면 Mobile Analytics Client SDK 로거 API를 사용하십시오.
   Mobile Analytics Client SDK 로깅 프레임워크에서는 다음과 같은 로그 레벨을 지원합니다. 이 레벨은 가장 간단한 로깅에서 가장 상세한 로깅 순으로 나열되어 있으며 권장 사용 가이드라인이 함께 제공됩니다.
    * FATAL - 복구할 수 없는 충돌 또는 정지가 발생한 경우 사용합니다. FATAL 레벨은 애플리케이션이 충돌할 때 사용자에게 표시되는 복구할 수 없는 오류를 로깅하는 데 사용하도록 예약되어 있습니다.
    * ERROR - 예상치 못한 예외 또는 예상치 못한 네트워크 프로토콜 오류에 사용합니다.
    * WARN - 더 이상 사용되지 않는 API나 느린 네트워크 응답과 같이 중요한 오류로 간주되지 않는 사용 경고를 로깅하는 데 사용합니다.
    * INFO - 중요하지만 긴급하지는 않은 초기화 이벤트 및 기타 데이터를 보고하는 데 사용합니다.
    * DEBUG - 개발자가 애플리케이션 결함을 해결하는 데 도움이 되도록 디버그 명령문을 보고하는 데 사용합니다.
    로거 레벨이 DEBUG로 설정된 경우에는 Mobile Analytics 클라이언트 SDK 로그도 제공됩니다. 이 로그는 로그를 전송할 때 포함됩니다.
    * TRACE - 메소드 시작점 및 종료점에 사용됩니다.
   {: note}
   {: web}

   다음 코드 스니펫에서는 웹 앱의 샘플 로거 사용법을 보여줍니다.
   ```Javascript
    //Create a logger instance.  You can create multiple loggers to organize your logs as you wish
    
    var logger = ibmmfpfanalytics.logger.pkg("loggerName");
    // Log messages with different levels
    // Debug message for feature 1
    // Info message for feature 2
    logger.debug("debug message");
    logger.info("info message");

    // Send logs to the Mobile Analytics
    ibmmfpfanalytics.send();
   ```
   {: codeblock}
   {: web}
   
   웹 SDK의 경우 클라이언트에서 레벨을 설정할 수 없습니다. 서버 구성 프로파일을 검색하여 구성이 변경될 때까지 모든 로깅이 서버에 전송됩니다.
   {: note}
   {: web}

6. 사용자 참여 패턴(새로운 사용자 대 돌아온 사용자)을 이해하려면 사용자 ID를 애플리케이션 세션과 연관시켜야 합니다.  이 작업은 다음 API를 호출하여 수행할 수 있습니다.
    ```Javascript
    ibmmfpfanalytics.setUserContext("userName or userIdentity");
    ```
    {: codeblock}
    {: web}

    로컬에서 캡처되고 저장된 모든 분석 데이터는 ibmmfpfanalytics.send() API가 호출될 때만 Mobile Analytics 서비스로 전송됩니다.
    {: web}

7.  호출된 HTTP API, 요청 수 및 평균 응답 시간과 같은 애플리케이션 HTTP 네트워크 상호작용 패턴을 이해하려면 Mobile Foundation Service Client SDK의 WLResourceRequest 클래스를 사용하여 HTTP 호출을 수행해야 합니다.  네트워크 이벤트를 캡처하기 위해 Analytics를 초기화한 경우에도 WLResourceRequest를 사용하면 Mobile Anayltics가 네트워크 호출에 후크하여 관련 데이터를 캡처할 수 있습니다.  다음과 같은 방법으로 HTTP 호출을 수행해야 합니다.
    ```Javascript
      var resourceRequest = new WL.ResourceRequest("url_path", WLResourceRequest.GET);
      resourceRequest.send().then(function(response) {
            console.log('handle success' + JSON.stringify(data));
        },function(error){
            console.log('handle failure: ' + error);
        });
    ```
    {: codeblock}
    {: web}

8.  Client SDK에서 기본적으로 지원하는 수준 이상으로 사용자 정의 분석을 정의하고 고유 분석 데이터를 정의하려면 사용자 정의 로깅 API를 사용할 수 있습니다.

    ```Javascript
        //custom data is sent with the addEvent method
        ibmmfpfanalytics.addEvent({'FromPage':'LoginPage'});

        //Send the captured custom data and log to the Mobile Analytics service
        ibmmfpfanalytics.send();
    ```
    {: codeblock}
    {: web}

    로깅된 사용자 정의 데이터를 Mobile Analytics 콘솔에서 정의할 수 있는 사용자 정의 차트에 표시하여 사용자 정의 인사이트를 도출할 수 있습니다.
    {: web}

## 다음 작업
{: #next_steps_analytics}

[여기](https://github.com/MobileFirst-Platform-Developer-Center/mfp-analytics-samples)에서 단순 샘플을 사용해 보십시오. 5분 미만에 샘플 애플리케이션을 실행하고 이 페이지에서 설명된 API가 수행하는 작업을 대략적으로 이해할 수 있습니다. Analytics 콘솔에서 분석이 여러 다른 인사이트로 표시되는 방법도 알 수 있습니다.  

Mobile Foundation Analytics Service에서는 개발자가 분석 데이터 가져오기(POST) 및 내보내기(GET)를 수행하는 데 도움이 되도록 REST API를 제공합니다.

[여기](https://mobile-analytics-dashboard.ng.bluemix.net/analytics-service/)에서 Swagger 문서의 분석 REST API를 사용해 보십시오.




