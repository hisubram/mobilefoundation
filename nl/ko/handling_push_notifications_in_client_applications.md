---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-06"

keywords: push notifications, notifications, set up android app for notification, set up iOS app for notification, set up cordova app for notification, set up windows app for notification

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
{:xml: .ph data-hd-programlang='xml'}
{:windows: .ph data-hd-programlang='Windows'}


# 클라이언트 애플리케이션에서 푸시 알림 처리
{: #handling_push_notifications_in_client_applications}

iOS, Android 및 Windows 네이티브 기반 또는 Cordova 기반 애플리케이션이 수신 푸시 알림을 수신하고 표시할 수 있으려면 먼저 애플리케이션을 설정하고 API를 구현해야 합니다.
{: shortdesc}

클라이언트 애플리케이션에서 수신 푸시 알림을 처리하는 방법을 학습하려면 다음 주제를 참조하십시오.

### Android에서 푸시 알림 처리
{: #handling_push_notifications_in_android }
{: android}
Android 애플리케이션이 수신된 푸시 알림을 처리할 수 있으려면 먼저 Google Play Services에 대한 지원을 구성해야 합니다. 애플리케이션이 구성된 후 {{ site.data.keyword.mobilefirst_notm }} 제공 알림 API를 사용하여 디바이스를 등록 및 등록 취소하고 태그를 구독 및 구독 해지할 수 있습니다. 이 튜토리얼에서는 Android 애플리케이션에서 푸시 알림을 처리하는 방법에 대해 학습합니다.
{: android}

**전제조건:**
{: android}
* 로컬로 실행되는 {{ site.data.keyword.mfserver_short_notm }} 또는 원격으로 실행 중인 {{ site.data.keyword.mfserver_short_notm }}
* 개발자 워크스테이션에 설치된 {{ site.data.keyword.mobilefirst_notm  }} CLI
{: android}

#### 알림 구성
{: #notifications-configuration }
{: android}

새 Android Studio 프로젝트를 작성하거나 기존 프로젝트를 사용하십시오.  
{{ site.data.keyword.mobilefirst_notm }} 네이티브 Android SDK가 프로젝트에 아직 없는 경우 [Android 애플리케이션에 {{ site.data.keyword.mobilefoundation_short }} SDK 추가](https://cloud.ibm.com/docs/services/mobilefoundation/add_sdk_to_app.html#add_sdk_to_app) 튜토리얼의 지시사항을 따르십시오.
{: android}

##### 프로젝트 설정
{: #project-setup }
{: android}

1. **Android → Gradle 스크립트**에서 **build.gradle (Module: app)** 파일을 선택하고 다음 행을 `dependencies`에 추가하십시오.
{: android}

    ```bash
    com.google.android.gms:play-services-gcm:9.0.2
    ```
    {: codeblock}
    {: android}

    최신 Play Services 버전(현재 9.2.0)을 사용하지 못하게 하는 [알려진 Google 결함](https://code.google.com/p/android/issues/detail?id=212879)이 있습니다. 더 낮은 버전을 사용하십시오.
    {: note}
    {: android}

    그리고 다음을 추가하십시오.
    ```xml
    compile group: 'com.ibm.mobile.foundation',
             name: 'ibmmobilefirstplatformfoundationpush',
             version: '8.0.+',
             ext: 'aar',
             transitive: true
    ```
    {: codeblock}
    {: android}

    또는 단일 행에 다음을 추가하십시오.
    ```xml
    compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundationpush:8.0.+'
    ```
    {: codeblock}
    {: android}

1. **Android → 앱 → Manifest**에서 `AndroidManifest.xml` 파일을 여십시오.
    * `manifest` 태그의 맨 위에 다음과 같은 권한을 추가하십시오.

        ```xml
        <!-- Permissions -->
        <uses-permission android:name="android.permission.WAKE_LOCK" />

        <!-- GCM Permissions -->
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
        <permission
    	     android:name="your.application.package.name.permission.C2D_MESSAGE"
    	     android:protectionLevel="signature" />
        ```
        {: codeblock}
        {: android}

    * `application` 태그에 다음을 추가하십시오.

        ```xml
        <!-- GCM Receiver -->
        <receiver
             android:name="com.google.android.gms.gcm.GcmReceiver"
             android:exported="true"
             android:permission="com.google.android.c2dm.permission.SEND">
             <intent-filter>
                 <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                 <category android:name="your.application.package.name" />
             </intent-filter>
        </receiver>

        <!-- MFPPush Intent Service -->
        <service
              android:name="com.ibm.mobilefirstplatform.clientsdk.android.push.api.MFPPushIntentService"
              android:exported="false">
              <intent-filter>
                  <action android:name="com.google.android.c2dm.intent.RECEIVE" />
              </intent-filter>
        </service>

        <!-- MFPPush Instance ID Listener Service -->
        <service
              android:name="com.ibm.mobilefirstplatform.clientsdk.android.push.api.MFPPushInstanceIDListenerService"
              android:exported="false">
              <intent-filter>
                  <action android:name="com.google.android.gms.iid.InstanceID" />
              </intent-filter>
        </service>

        <activity android:name="com.ibm.mobilefirstplatform.clientsdk.android.push.api.MFPPushNotificationHandler"
             android:theme="@android:style/Theme.NoDisplay"/>
 	    ```
   {: codeblock}
      {: android}

      `your.application.package.name`을 애플리케이션의 실제 패키지 이름으로 대체해야 합니다.
      {: note}
      {: android}

    * 애플리케이션의 활동에 다음 `intent-filter`를 추가하십시오.
        ```xml
        <intent-filter>
            <action android:name="your.application.package.name.IBMPushNotification" />
            <category android:name="android.intent.category.DEFAULT" />
        </intent-filter>
        ```
        {: codeblock}
        {: android}

#### 알림 API
{: #notifications-api }
{: android}

##### MFPPush 인스턴스
{: #mfppush-instance }
{: android}

모든 API 호출은 `MFPPush`의 인스턴스에서 호출되어야 합니다. 이는 `private MFPPush push = MFPPush.getInstance();`와 같은 클래스 레벨 필드를 작성한 후 클래스 전체에서 `push.<api-call>`을 호출하여 수행될 수 있습니다.
{: android}

또는 푸시 API 메소드에 액세스해야 하는 각 인스턴스에 대해 `MFPPush.getInstance().<api_call>`을 호출할 수 있습니다.
{: android}

##### 인증 확인 핸들러
{: #challenge-handlers }
{: android}

`push.mobileclient` 범위가 **보안 검사**에 맵핑된 경우 푸시 API를 사용하기 전에 일치하는 **인증 확인 핸들러**가 존재하며 등록되어 있는지 확인해야 합니다.
{: android}

[인증 정보 유효성 검증 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/credentials-validation/android/) 튜토리얼에서 인증 확인 핸들러에 대해 자세히 알아보십시오.
{: note}
{: android}

##### 클라이언트 측
{: #client-side }
{: android}

| Java 메소드 | 설명 |
|-----------------------------------------------------------------------------------|-------------------------------------------------------------------------|
| [`initialize(Context context);`](#initialization) | 제공된 컨텍스트에 대한 MFPPush를 초기화합니다. |
| [`isPushSupported();`](#is-push-supported) | 디바이스가 푸시 알림을 지원하는지 확인합니다. |
| [`registerDevice(JSONObject, MFPPushResponseListener);`](#register-device) | 디바이스를 푸시 알림 서비스에 등록합니다. |
| [`getTags(MFPPushResponseListener)`](#get-tags) | 푸시 알림 서비스 인스턴스에서 사용 가능한 태그를 검색합니다. |
| [`subscribe(String[] tagNames, MFPPushResponseListener)`](#subscribe) | 디바이스가 지정된 태그를 구독하도록 합니다. |
| [`getSubscriptions(MFPPushResponseListener)`](#get-subscriptions) | 현재 디바이스가 구독하는 모든 태그를 검색합니다. |
| [`unsubscribe(String[] tagNames, MFPPushResponseListener)`](#unsubscribe) | 특정 태그의 구독을 해지합니다. |
| [`unregisterDevice(MFPPushResponseListener)`](#unregister) | 푸시 알림 서비스에서 디바이스의 등록을 취소합니다. |
{: android}

###### 초기화
{: #initialization }
{: android}

클라이언트 애플리케이션이 올바른 애플리케이션 컨텍스트를 사용하여 MFPPush 서비스에 연결하는 데 필요합니다.
{: android}

* 다른 MFPPush API를 사용하기 전에 먼저 API 메소드를 호출해야 합니다.
* 수신된 푸시 알림을 처리하도록 콜백 함수를 등록합니다.
{: android}

```java
MFPPush.getInstance().initialize(this);
```
{: codeblock}
{: android}

###### 푸시가 지원되는지 여부
{: #is-push-supported }
{: android}

디바이스가 푸시 알림을 지원하는지 확인합니다.
{: android}

```java
Boolean isSupported = MFPPush.getInstance().isPushSupported();

if (isSupported ) {
    // Push is supported
} else {
    // Push is not supported
}
```
{: codeblock}
{: android}

###### 디바이스 등록
{: #register-device }
{: android}

디바이스를 푸시 알림 서비스에 등록합니다.
{: android}

```java
MFPPush.getInstance().registerDevice(null, new MFPPushResponseListener<String>() {
    @Override
    public void onSuccess(String s) {
        // Successfully registered
    }

    @Override
    public void onFailure(MFPPushException e) {
        // Registration failed with error
    }
});
```
{: codeblock}
{: android}

###### 태그 가져오기
{: #get-tags }
{: android}

푸시 알림 서비스에서 사용 가능한 모든 태그를 검색합니다.
{: android}

```java
MFPPush.getInstance().getTags(new MFPPushResponseListener<List<String>>() {
    @Override
    public void onSuccess(List<String> strings) {
        // Successfully retrieved tags as list of strings
    }

    @Override
    public void onFailure(MFPPushException e) {
        // Failed to receive tags with error
    }
});
```
{: codeblock}
{: android}

###### 구독
{: #subscribe }
{: android}

원하는 태그를 구독합니다.
{: android}

```java
String[] tags = {"Tag 1", "Tag 2"};

MFPPush.getInstance().subscribe(tags, new MFPPushResponseListener<String[]>() {
    @Override
    public void onSuccess(String[] strings) {
        // Subscribed successfully
    }

    @Override
    public void onFailure(MFPPushException e) {
        // Failed to subscribe
    }
});
```
{: codeblock}
{: android}

###### 구독 가져오기
{: #get-subscriptions }
{: android}

현재 디바이스가 구독하는 태그를 검색합니다.
{: android}

```java
MFPPush.getInstance().getSubscriptions(new MFPPushResponseListener<List<String>>() {
    @Override
    public void onSuccess(List<String> strings) {
        // Successfully received subscriptions as list of strings
    }

    @Override
    public void onFailure(MFPPushException e) {
        // Failed to retrieve subscriptions with error
    }
});
```
{: codeblock}
{: android}

###### 구독 해지
{: #unsubscribe }
{: android}

태그의 구독을 해지합니다.
{: android}

```java
String[] tags = {"Tag 1", "Tag 2"};

MFPPush.getInstance().unsubscribe(tags, new MFPPushResponseListener<String[]>() {
    @Override
    public void onSuccess(String[] strings) {
        // Unsubscribed successfully
    }

    @Override
    public void onFailure(MFPPushException e) {
        // Failed to unsubscribe
    }
});
```
{: codeblock}
{: android}

###### 등록 취소
{: #unregister }
{: android}

푸시 알림 서비스 인스턴스에서 디바이스의 등록을 취소합니다.
{: android}

```java
MFPPush.getInstance().unregisterDevice(new MFPPushResponseListener<String>() {
    @Override
    public void onSuccess(String s) {
        disableButtons();
        // Unregistered successfully
    }

    @Override
    public void onFailure(MFPPushException e) {
        // Failed to unregister
    }
});
```
{: codeblock}
{: android}

#### 푸시 알림 처리
{: #handling-a-push-notification }
{: android}

푸시 알림을 처리하려면 `MFPPushNotificationListener`를 설정해야 합니다. 이는 다음 메소드 중 하나를 구현하여 수행할 수 있습니다.
{: android}

##### 옵션 1
{: #option-one }
{: android}

푸시 알림을 처리할 활동에서 다음을 수행하십시오.
{: android}

1. 클래스 선언에 `implements MFPPushNofiticationListener`를 추가하십시오.
2. `onCreate` 메소드에서 `MFPPush.getInstance().listen(this)`를 호출하여 클래스를 리스너로 설정하십시오.
2. 그런 다음, 다음과 같은 *필수* 메소드를 추가해야 합니다.
    ```java
    @Override
    public void onReceive(MFPSimplePushNotification mfpSimplePushNotification) {
         // Handle push notification here
    }
    ```
    {: codeblock}
    {: android}

3. 이 메소드에서 `MFPSimplePushNotification`을 수신하고 원하는 동작에 대한 알림을 처리할 수 있습니다.
{: android}

##### 옵션 2
{: #option-two }
{: android}

아래에 설명된 대로 `MFPPush`의 인스턴스에서 `listen(new MFPPushNofiticationListener())`를 호출하여 리스너를 작성하십시오.
```java
MFPPush.getInstance().listen(new MFPPushNotificationListener() {
    @Override
    public void onReceive(MFPSimplePushNotification mfpSimplePushNotification) {
        // Handle push notification here
    }
});
```
{: codeblock}
{: android}

#### Android의 클라이언트 앱을 FCM으로 마이그레이션
{: #migrate-to-fcm }
{: android}

GCM(Google Cloud Messaging)은 [더 이상 사용되지 않으며](https://developers.google.com/cloud-messaging/faq) FCM(Firebase Cloud Messaging)과 통합되었습니다. Google은 2019년 4월까지 대부분의 GCM 서비스를 중단합니다.
{: android}

GCM 프로젝트를 사용 중인 경우 [Android의 GCM 클라이언트 앱을 FCM으로 마이그레이션](https://developers.google.com/cloud-messaging/android/android-migrate-fcm)하십시오.
{: android}

현재 GCM 서비스를 사용 중인 기존 애플리케이션은 원래 상태로 계속 작동합니다. 푸시 알림 서비스는 FCM 엔드포인트를 사용하도록 업데이트되었으므로 앞으로 모든 새 애플리케이션이 FCM을 사용해야 합니다.
{: android}

FCM으로 마이그레이션한 후 이전 GCM 인증 정보 대신 FCM 인증 정보를 사용하도록 프로젝트를 업데이트하십시오.
{: note}
{: android}

##### FCM 프로젝트 설정
{: #fcm_project_setup}
{: android}

FCM에서 애플리케이션을 설정하는 작업은 이전 GCM 모델과는 약간 다릅니다.
{: android}

1. 알림 제공자 인증 정보를 확보하고 FCM 프로젝트를 작성한 후 Android 애플리케이션에 동일한 사항을 추가하십시오. 애플리케이션의 패키지 이름을 `com.ibm.mobilefirstplatform.clientsdk.android.push`로 포함하십시오. `google-services.json` 파일 생성을 완료하는 단계까지 [여기의 문서](https://cloud.ibm.com/docs/services/mobilepush/push_step_1.html#push_step_1_android)를 참조하십시오.
   {: android}
2. Gradle 파일을 구성하십시오. 앱의 `build.gradle` 파일에 다음을 추가하십시오.
    ```xml
    dependencies {
       ......
       compile 'com.google.firebase:firebase-messaging:10.2.6'
       .....
    }

    apply plugin: 'com.google.gms.google-services'
    ```
    {: codeblock}
    {: android}

    - 루트 build.gradle의 `buildscript` 섹션에 다음 종속 항목을 추가하십시오.
      `classpath 'com.google.gms:google-services:3.0.0'`
      {: codeblock}
    {: android}

    - build.gradle 파일에서 다음 GCM 플러그인을 제거하십시오. `compile com.google.android.gms:play-services-gcm:+`
     {: android}
 3. AndroidManifest 파일을 구성하십시오. `AndroidManifest.xml`을 다음과 같이 변경해야 합니다.
    {: android}

    **다음 항목을 제거해야 합니다.**
    {: android}
    ```xml
        <receiver android:exported="true" android:name="com.google.android.gms.gcm.GcmReceiver" android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="your.application.package.name" />
            </intent-filter>
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
                <category android:name="your.application.package.name" />
            </intent-filter>
        </receiver>  

        <service android:exported="false" android:name="com.ibm.mobilefirstplatform.clientsdk.android.push.api.MFPPushInstanceIDListenerService">
            <intent-filter>
                <action android:name="com.google.android.gms.iid.InstanceID" />
            </intent-filter>
        </service>

        <uses-permission android:name="your.application.package.name.permission.C2D_MESSAGE" />
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
    ```
    {: codeblock}
    {: android}

    **다음 항목을 수정해야 합니다.**
    {: android}

    ```xml
        <service android:exported="true" android:name="com.ibm.mobilefirstplatform.clientsdk.android.push.api.MFPPushIntentService">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
            </intent-filter>
        </service>
    ```
    {: codeblock}
    {: android}

    **이 항목을 다음으로 수정하십시오.**
    {: android}

    ```xml
        <service android:exported="true" android:name="com.ibm.mobilefirstplatform.clientsdk.android.push.api.MFPPushIntentService">
            <intent-filter>
                <action android:name="com.google.firebase.MESSAGING_EVENT" />
            </intent-filter>
        </service>
    ```
    {: codeblock}
    {: android}

    **다음 항목을 추가하십시오.**
    {: android}

    ```xml
        <service android:name="com.ibm.mobilefirstplatform.clientsdk.android.push.api.MFPPush"
                android:exported="true">
                <intent-filter>
                    <action android:name="com.google.firebase.INSTANCE_ID_EVENT" />
                </intent-filter>
        </service>
    ```
    {: codeblock}
    {: android}

 4. Android Studio에서 앱을 여십시오. **1단계**에서 작성한 `google-services.json` 파일을 앱 디렉토리 내부에 복사하십시오. `google-service.json` 파일에는 사용자가 추가한 패키지 이름이 포함되어 있습니다.		
 5. SDK를 컴파일하십시오. 애플리케이션을 빌드하십시오.
{: android}

### iOS에서 푸시 알림 처리
{: #handling_push_notifications_in_ios }
{: ios}

{{ site.data.keyword.mobilefirst_notm }} 제공 알림 API를 사용하여 디바이스를 등록 및 등록 취소하고 태그를 구독 및 구독 해지할 수 있습니다. 이 튜토리얼에서는 Swift를 사용하여 iOS 애플리케이션에서 푸시 알림을 처리하는 방법에 대해 학습합니다.
{: shortdesc}
{: ios}

자동 또는 대화식 알림에 대한 정보는 다음을 참조하십시오.


* [자동 알림](/docs/services/mobilefoundation?topic=mobilefoundation-silent_notifications#silent_notifications)
* [대화식 알림](/docs/services/mobilefoundation?topic=mobilefoundation-interactive_notifications#interactive_notifications)
{: ios}

**전제조건:**
{: ios}

* 로컬로 실행되는 {{ site.data.keyword.mfserver_short }} 또는 원격으로 실행 중인 {{ site.data.keyword.mfserver_short }}
* 개발자 워크스테이션에 설치된 {{ site.data.keyword.mfp_cli_long_notm }}
{: ios}

#### 알림 구성
{: #notifications-configuration }
{: ios}

새 Xcode 프로젝트를 작성하거나 기존 프로젝트를 사용하십시오.
{{ site.data.keyword.mobilefirst_notm }} 네이티브 iOS SDK가 프로젝트에 아직 없는 경우 [iOS 애플리케이션에 {{ site.data.keyword.mobilefoundation_short }} SDK 추가](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/ios/) 튜토리얼의 지시사항을 따르십시오.
{: ios}

#### 푸시 SDK 추가
{: #adding-the-push-sdk }
{: ios}

1. 프로젝트의 기본 **podfile**을 열고 다음 행을 추가하십시오.
    ```xml
    use_frameworks!

    platform :ios, 8.0
    target "Xcode-project-target" do
         pod 'IBMMobileFirstPlatformFoundation'
         pod 'IBMMobileFirstPlatformFoundationPush'
    end

    post_install do |installer|
         workDir = Dir.pwd

         installer.pods_project.targets.each do |target|
             debugXcconfigFilename = "#{workDir}/Pods/Target Support Files/#{target}/#{target}.debug.xcconfig"
             xcconfig = File.read(debugXcconfigFilename)
             newXcconfig = xcconfig.gsub(/HEADER_SEARCH_PATHS = .*/, "HEADER_SEARCH_PATHS = ")
             File.open(debugXcconfigFilename, "w") { |file| file << newXcconfig }

             releaseXcconfigFilename = "#{workDir}/Pods/Target Support Files/#{target}/#{target}.release.xcconfig"
             xcconfig = File.read(releaseXcconfigFilename)
             newXcconfig = xcconfig.gsub(/HEADER_SEARCH_PATHS = .*/, "HEADER_SEARCH_PATHS = ")
             File.open(releaseXcconfigFilename, "w") { |file| file << newXcconfig }
         end
    end
    ```
    {: codeblock}
    {: ios}

    - **Xcode-project-target**을 Xcode 프로젝트의 대상 이름으로 대체하십시오.
2. **podfile**을 저장한 후 닫으십시오.
3. **명령행** 창에서 프로젝트의 루트 폴더로 이동하십시오.
4. `pod install` 명령을 실행하십시오.
5. **.xcworkspace** 파일을 사용하여 프로젝트를 여십시오.
{: ios}

#### 알림 API
{: #notifications-api }
{: ios}

##### MFPPush 인스턴스
{: #mfppush-instance }
{: ios}

모든 API 호출은 `MFPPush`의 인스턴스에서 호출되어야 합니다. 이는 보기 제어기에서 `var`을 사용한 후(예: `var push = MFPPush.sharedInstance();`) 보기 제어기 전체에서 `push.methodName()`을 호출하여 수행될 수 있습니다.
{: ios}

또는 푸시 API 메소드에 액세스해야 하는 각 인스턴스에 대해 `MFPPush.sharedInstance().methodName()`을 호출할 수 있습니다.
{: ios}

#### 인증 확인 핸들러
{: #challenge-handlers }
{: ios}

`push.mobileclient` 범위가 **보안 검사**에 맵핑된 경우 푸시 API를 사용하기 전에 일치하는 **인증 확인 핸들러**가 존재하며 등록되어 있는지 확인해야 합니다.
{: ios}

[인증 정보 유효성 검증 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/credentials-validation/ios/) 튜토리얼에서 인증 확인 핸들러에 대해 자세히 알아보십시오.
{: note}
{: ios}

#### 클라이언트 측
{: #client-side }
{: ios}

| Swift 메소드 | 설명  |
|---------------|--------------|
| [`initialize()`](#initialization) | 제공된 컨텍스트에 대한 MFPPush를 초기화합니다. |
| [`isPushSupported()`](#is-push-supported) | 디바이스가 푸시 알림을 지원하는지 확인합니다. |
| [`registerDevice(completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#register-device--send-device-token) | 디바이스를 푸시 알림 서비스에 등록합니다. |
| [`sendDeviceToken(deviceToken: NSData!)`](#register-device--send-device-token) | 디바이스 토큰을 서버에 전송합니다. |
| [`getTags(completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#get-tags) | 푸시 알림 서비스 인스턴스에서 사용 가능한 태그를 검색합니다. |
| [`subscribe(tagsArray: [AnyObject], completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#subscribe) | 디바이스가 지정된 태그를 구독하도록 합니다. |
| [`getSubscriptions(completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#get-subscriptions)  | 현재 디바이스가 구독하는 모든 태그를 검색합니다. |
| [`unsubscribe(tagsArray: [AnyObject], completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#unsubscribe) | 특정 태그의 구독을 해지합니다. |
| [`unregisterDevice(completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#unregister) | 푸시 알림 서비스에서 디바이스의 등록을 취소합니다. |
{: ios}

##### 초기화
{: #initialization }
{: ios}

클라이언트 애플리케이션이 MFPPush 서비스에 연결하려면 초기화가 필요합니다.
{: ios}

* 다른 MFPPush API를 사용하기 전에 먼저 `initialize` 메소드를 호출해야 합니다.
* 이는 수신된 푸시 알림을 처리하도록 콜백 함수를 등록합니다.
{: ios}

```swift
MFPPush.sharedInstance().initialize();
```
{: codeblock}
{: ios}

##### 푸시가 지원되는지 여부
{: #is-push-supported }
{: ios}

디바이스가 푸시 알림을 지원하는지 확인합니다.
{: ios}

```swift
let isPushSupported: Bool = MFPPush.sharedInstance().isPushSupported()

if isPushSupported {
    // Push is supported
} else {
    // Push is not supported
}
```
{: codeblock}
{: ios}

##### 디바이스 등록 및 디바이스 토큰 전송
{: #register-device--send-device-token }
{: ios}

디바이스를 푸시 알림 서비스에 등록합니다.
{: ios}

```swift
MFPPush.sharedInstance().registerDevice(nil) { (response, error) -> Void in
    if error == nil {
        self.enableButtons()
        self.showAlert("Registered successfully")
        print(response?.description ?? "")
    } else {
        self.showAlert("Registrations failed.  Error \(error?.localizedDescription)")
        print(error?.localizedDescription ?? "")
    }
}
```
{: codeblock}
{: ios}

<!--`options` = `[NSObject : AnyObject]` which is an optional parameter that is a dictionary of options to be passed with your register request, sends the device token to the server to register the device with its unique identifier.-->

```swift
MFPPush.sharedInstance().sendDeviceToken(deviceToken)
```
{: codeblock}
{: ios}

이는 일반적으로 `didRegisterForRemoteNotificationsWithDeviceToken` 메소드의 **AppDelegate**에서 호출됩니다.
{: note}
{: ios}

##### 태그 가져오기
{: #get-tags }
{: ios}

푸시 알림 서비스에서 사용 가능한 모든 태그를 검색합니다.
{: ios}

```swift
MFPPush.sharedInstance().getTags { (response, error) -> Void in
    if error == nil {
        print("The response is: \(response)")
        print("The response text is \(response?.responseText)")
        if response?.availableTags().isEmpty == true {
            self.tagsArray = []
            self.showAlert("There are no available tags")
        } else {
            self.tagsArray = response!.availableTags() as! [String]
            self.showAlert(String(describing: self.tagsArray))
            print("Tags response: \(response)")
        }
    } else {
        self.showAlert("Error \(error?.localizedDescription)")
        print("Error \(error?.localizedDescription)")
    }
}
```
{: codeblock}
{: ios}

##### 구독
{: #subscribe }
{: ios}

원하는 태그를 구독합니다.
{: ios}

```swift
var tagsArray: [String] = ["Tag 1", "Tag 2"]

MFPPush.sharedInstance().subscribe(self.tagsArray) { (response, error)  -> Void in
    if error == nil {
        self.showAlert("Subscribed successfully")
        print("Subscribed successfully response: \(response)")
    } else {
        self.showAlert("Failed to subscribe")
        print("Error \(error?.localizedDescription)")
    }
}
```
{: codeblock}
{: ios}

##### 구독 가져오기
{: #get-subscriptions }
{: ios}

현재 디바이스가 구독하는 태그를 검색합니다.
{: ios}

```swift
MFPPush.sharedInstance().getSubscriptions { (response, error) -> Void in
   if error == nil {
       var tags = [String]()
       let json = (response?.responseJSON)! as [AnyHashable: Any]
       let subscriptions = json["subscriptions"] as? [[String: AnyObject]]
       for tag in subscriptions! {
           if let tagName = tag["tagName"] as? String {
               print("tagName: \(tagName)")
               tags.append(tagName)
           }
       }
       self.showAlert(String(describing: tags))
   } else {
       self.showAlert("Error \(error?.localizedDescription)")
       print("Error \(error?.localizedDescription)")
   }
}
```
{: codeblock}
{: ios}

##### 구독 해지
{: #unsubscribe }
{: ios}

태그의 구독을 해지합니다.
{: ios}

```swift
var tags: [String] = {"Tag 1", "Tag 2"};

// Unsubscribe from tags
MFPPush.sharedInstance().unsubscribe(self.tagsArray) { (response, error)  -> Void in
    if error == nil {
        self.showAlert("Unsubscribed successfully")
        print(String(describing: response?.description))
    } else {
        self.showAlert("Error \(error?.localizedDescription)")
        print("Error \(error?.localizedDescription)")
    }
}
```
{: codeblock}
{: ios}

##### 등록 취소
{: #unregister }
{: ios}

푸시 알림 서비스 인스턴스에서 디바이스의 등록을 취소합니다.
{: ios}

```swift
MFPPush.sharedInstance().unregisterDevice { (response, error)  -> Void in
   if error == nil {
       // Disable buttons
       self.disableButtons()
       self.showAlert("Unregistered successfully")
       print("Subscribed successfully response: \(response)")
   } else {
       self.showAlert("Error \(error?.localizedDescription)")
       print("Error \(error?.localizedDescription)")
   }
}
```
{: codeblock}
{: ios}

#### 푸시 알림 처리
{: #handling-a-push-notification }
{: ios}

푸시 알림은 네이티브 iOS 프레임워크에서 직접 처리됩니다. 애플리케이션 라이프사이클에 따라 iOS 프레임워크에서 다양한 메소드가 호출됩니다.
{: ios}

예를 들어, 애플리케이션이 실행되는 동안 단순 알림이 수신되는 경우 **AppDelegate**의 `didReceiveRemoteNotification`이 트리거됩니다.
{: ios}

```swift
func application(_ application: UIApplication, didReceiveRemoteNotification userInfo: [AnyHashable: Any]) {
    print("Received Notification in didReceiveRemoteNotification \(userInfo)")
    // display the alert body
      if let notification = userInfo["aps"] as? NSDictionary,
        let alert = notification["alert"] as? NSDictionary,
        let body = alert["body"] as? String {
          showAlert(body)
        }
}
```
{: codeblock}
{: ios}

[Apple 문서 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html#//apple_ref/doc/uid/TP40008194-CH8-SW1)에서 iOS의 알림 처리에 대해 자세히 알아보십시오.
{: note}
{: ios}

### Cordova에서 푸시 알림 처리
{: #handling_push_notifications_in_cordova }
{: cordova}

OS, Android 및 Windows Cordova 애플리케이션이 푸시 알림을 수신하고 표시할 수 있으려면 먼저 **cordova-plugin-mfp-push** Cordova 플러그인을 Cordova 프로젝트에 추가해야 합니다. 애플리케이션이 구성된 후 {{ site.data.keyword.mobilefirst_notm }} 제공 알림 API를 사용하여 디바이스를 등록 및 등록 취소하고 태그를 구독 및 구독 해지하며 알림을 처리할 수 있습니다. 이 튜토리얼에서는 Cordova 애플리케이션에서 푸시 알림을 처리하는 방법에 대해 학습합니다.
{: shortdesc}
{: cordova}

결함으로 인해 Cordova 애플리케이션에서는 현재 인증된 알림이 **지원되지 않습니다**. 그러나 `WLAuthorizationManager.obtainAccessToken("push.mobileclient").then( ... );`으로 각 `MFPPush` API 호출을 랩핑할 수 있는 임시 해결책이 제공됩니다.
{: note}
{: cordova}

iOS의 자동 또는 대화식 알림에 대한 정보는 다음을 참조하십시오.
{: cordova}

* [자동 알림](/docs/services/mobilefoundation?topic=mobilefoundation-silent_notifications#silent_notifications)
* [대화식 알림](/docs/services/mobilefoundation?topic=mobilefoundation-interactive_notifications#interactive_notifications)
{: cordova}

**전제조건:**
{: cordova}

* 로컬로 실행되는 {{ site.data.keyword.mfserver_short }} 또는 원격으로 실행 중인 {{ site.data.keyword.mfserver_short }}
* 개발자 워크스테이션에 설치된 {{ site.data.keyword.mfp_cli_long_notm }}
* 개발자 워크스테이션에 설치된 Cordova CLI
{: cordova}

#### 알림 구성
{: #notifications-configuration }
{: cordova}

새 Cordova 프로젝트를 작성하거나 기존 프로젝트를 사용하고 지원되는 플랫폼(iOS, Android, Windows) 중 하나 이상을 추가하십시오.
{: cordova}

{{ site.data.keyword.mobilefirst_notm }} Cordova SDK가 프로젝트에 아직 없는 경우 [Cordova 애플리케이션에 {{ site.data.keyword.mobilefirst_notm }} SDK 추가](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/cordova/) 튜토리얼의 지시사항을 따르십시오.
{: cordova}
{: note}

#### 푸시 플러그인 추가
{: #adding-the-push-plug-in }
{: cordova}

1. **명령행** 창에서 Cordova 프로젝트의 루트로 이동하십시오.  
2. 다음 명령을 실행하여 푸시 플러그인을 추가하십시오.
    ```bash
    cordova plugin add cordova-plugin-mfp-push
    ```
    {: codeblock}

3. 다음 명령을 실행하여 Cordova 프로젝트를 빌드하십시오.
    ```bash
    cordova build
    ```
    {: codeblock}
{: cordova}

#### iOS 플랫폼
{: #ios-platform }
{: cordova}

iOS 플랫폼에는 추가 단계가 필요합니다.  
{: cordova}

Xcode의 **기능** 화면에서 애플리케이션에 대한 푸시 알림을 사용으로 설정하십시오.
{: cordova}

애플리케이션에 대해 선택된 번들 ID는 이전에 Apple Developer 사이트에서 작성한 앱 ID와 일치해야 합니다.
{: important}
{: cordova}

![Xcode의 기능이 있는 위치의 이미지](images/push-capability.png)
{: cordova}

#### Android 플랫폼
{: #android-platform }
{: cordova}

Android 플랫폼에는 추가 단계가 필요합니다.  
{: cordova}

Android Studio에서 다음 `activity`를 `application` 태그에 추가하십시오.
```xml
<activity android:name="com.ibm.mobilefirstplatform.clientsdk.android.push.api.MFPPushNotificationHandler" android:theme="@android:style/Theme.NoDisplay"/>
```
{: codeblock}
{: cordova}

#### 알림 API
{: #notifications-api }
{: cordova}

##### 클라이언트 측
{: #client-side }
{: cordova}

| Javascript 함수 | 설명 |
| --- | --- |
| [`MFPPush.initialize(success, failure)`](#initialization) | MFPPush 인스턴스를 초기화합니다. |
| [`MFPPush.isPushSupported(success, failure)`](#is-push-supported) | 디바이스가 푸시 알림을 지원하는지 확인합니다. |
| [`MFPPush.registerDevice(options, success, failure)`](#register-device) | 디바이스를 푸시 알림 서비스에 등록합니다. |
| [`MFPPush.getTags(success, failure)`](#get-tags) | 푸시 알림 서비스 인스턴스에서 사용 가능한 모든 태그를 검색합니다. |
| [`MFPPush.subscribe(tag, success, failure)`](#subscribe) | 특정 태그를 구독합니다. |
| [`MFPPush.getSubsciptions(success, failure)`](#get-subscriptions) | 현재 디바이스가 구독하는 태그를 검색합니다. |
| [`MFPPush.unsubscribe(tag, success, failure)`](#unsubscribe) | 특정 태그의 구독을 해지합니다. |
| [`MFPPush.unregisterDevice(success, failure)`](#unregister) | 푸시 알림 서비스에서 디바이스의 등록을 취소합니다. |
{: cordova}

##### API 구현
{: #api-implementation }
{: cordova}

###### 초기화
{: #initialization }
{: cordova}

**MFPPush** 인스턴스를 초기화합니다.
{: cordova}

- 클라이언트 애플리케이션이 올바른 애플리케이션 컨텍스트를 사용하여 MFPPush 서비스에 연결하는 데 필요합니다.   
- 다른 MFPPush API를 사용하기 전에 먼저 API 메소드를 호출해야 합니다.
- 수신된 푸시 알림을 처리하도록 콜백 함수를 등록합니다.
{: cordova}

```javascript
MFPPush.initialize (
    function(successResponse) {
        alert("Successfully intialized");
        MFPPush.registerNotificationsCallback(notificationReceived);
    },
    function(failureResponse) {
        alert("Failed to initialize");
    }
);
```
{: codeblock}
{: cordova}

###### 푸시가 지원되는지 여부
{: #is-push-supported }
{: cordova}

디바이스가 푸시 알림을 지원하는지 확인합니다.
{: cordova}

```javascript
MFPPush.isPushSupported (
    function(successResponse) {
        alert("Push Supported: " + successResponse);
    },
    function(failureResponse) {
        alert("Failed to get push support status");
    }
);
```
{: codeblock}
{: cordova}

###### 디바이스 등록
{: #register-device }
{: cordova}

디바이스를 푸시 알림 서비스에 등록합니다. 옵션이 필요하지 않은 경우 options를 `null`로 설정할 수 있습니다.
{: cordova}

```javascript
var options = { };
MFPPush.registerDevice(
    options,
    function(successResponse) {
        alert("Successfully registered");
    },
    function(failureResponse) {
        alert("Failed to register");
    }
);
```
{: codeblock}
{: cordova}

###### 태그 가져오기
{: #get-tags }
{: cordova}

푸시 알림 서비스에서 사용 가능한 모든 태그를 검색합니다.
{: cordova}

```javascript
MFPPush.getTags (
    function(tags) {
        alert(JSON.stringify(tags));
},
    function() {
        alert("Failed to get tags");
    }
);
```
{: codeblock}
{: cordova}

###### 구독
{: #subscribe }
{: cordova}

원하는 태그를 구독합니다.
{: cordova}

```javascript
var tags = ['sample-tag1','sample-tag2'];

MFPPush.subscribe(
    tags,
    function(tags) {
        alert("Subscribed successfully");
    },
    function() {
        alert("Failed to subscribe");
    }
);
```
{: codeblock}
{: cordova}

###### 구독 가져오기
{: #get-subscriptions }
{: cordova}

현재 디바이스가 구독하는 태그를 검색합니다.
{: cordova}

```javascript
MFPPush.getSubscriptions (
    function(subscriptions) {
        alert(JSON.stringify(subscriptions));
    },
    function() {
        alert("Failed to get subscriptions");
    }
);
```
{: codeblock}
{: cordova}

###### 구독 해지
{: #unsubscribe }
{: cordova}

태그의 구독을 해지합니다.
{: cordova}

```javascript
var tags = ['sample-tag1','sample-tag2'];

MFPPush.unsubscribe(
    tags,
    function(tags) {
        alert("Unsubscribed successfully");
    },
    function() {
        alert("Failed to unsubscribe");
    }
);
```
{: codeblock}
{: cordova}

###### 등록 취소
{: #unregister }
{: cordova}

푸시 알림 서비스 인스턴스에서 디바이스의 등록을 취소합니다.
{: cordova}

```javascript
MFPPush.unregisterDevice(
    function(successResponse) {
        alert("Unregistered successfully");
    },
    function() {
        alert("Failed to unregister");
    }
);
```
{: codeblock}
{: cordova}

#### 푸시 알림 처리
{: #handling-a-push-notification }
{: cordova}

등록된 콜백 함수에서 해당 응답 오브젝트에 대해 조작을 수행하여 수신된 푸시 알림을 처리할 수 있습니다.
{: cordova}

```javascript
var notificationReceived = function(message) {
    alert(JSON.stringify(message));
};
```
{: codeblock}
{: cordova}

### Windows 8.1 Universal 및 Windows 10 UWP에서 푸시 알림 처리
{: #handling_push_notifications_in_windows }
{: windows}

{{ site.data.keyword.mobilefirst_notm }} 제공 알림 API를 사용하여 디바이스를 등록 및 등록 취소하고 태그를 구독 및 구독 해지할 수 있습니다. 이 튜토리얼에서는 C#을 사용하여 네이티브 Windows 8.1 Universal 및 Windows 10 UWP 애플리케이션에서 푸시 알림을 처리하는 방법에 대해 학습합니다.
{: windows}

**전제조건:**
{: windows}

* 로컬로 실행되는 {{ site.data.keyword.mfserver_short_notm }} 또는 원격으로 실행 중인 {{ site.data.keyword.mfserver_short_notm }}
* 개발자 워크스테이션에 설치된 {{ site.data.keyword.mobilefirst_notm  }} CLI
{: windows}

#### 알림 구성
{: #notifications-configuration }
{: windows}

새 Visual Studio 프로젝트를 작성하거나 기존 프로젝트를 사용하십시오.  
{: windows}

{{ site.data.keyword.mobilefirst_notm }} 네이티브 Windows SDK가 프로젝트에 아직 없는 경우 [Windows 애플리케이션에 {{ site.data.keyword.mobilefirst_notm }} SDK 추가](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/windows-8-10/) 튜토리얼의 지시사항을 따르십시오.
{: windows}

#### 푸시 SDK 추가
{: #adding-the-push-sdk }
{: windows}

1. 도구 → NuGet 패키지 관리자 → 패키지 관리 콘솔을 선택하십시오.
2. {{ site.data.keyword.mobilefirst_notm }} 푸시 컴포넌트를 설치할 프로젝트를 선택하십시오.
3. **Install-Package IBM.MobileFirstPlatformFoundationPush** 명령을 실행하여 {{ site.data.keyword.mobilefirst_notm }} 푸시 SDK를 추가하십시오.
{: windows}

#### 전제조건 WNS 구성
{: pre-requisite-wns-configuration }
{: windows}

1. 애플리케이션에 Toast 알림 기능이 있는지 확인하십시오. Package.appxmanifest에서 이를 사용으로 설정할 수 있습니다.
2. `Package Identity Name` 및 `Publisher`가 WNS에 등록된 값으로 업데이트되어야 합니다.
3. (선택사항) TemporaryKey.pfx 파일을 삭제하십시오.
{: windows}

#### 알림 API
{: #notifications-api }
{: windows}

##### MFPPush 인스턴스
{: #mfppush-instance }
{: windows}

모든 API 호출은 `MFPPush`의 인스턴스에서 호출되어야 합니다. 이는 `private MFPPush PushClient = MFPPush.GetInstance();`와 같은 변수를 작성한 후 클래스 전체에서 `PushClient.methodName()`을 호출하여 수행될 수 있습니다.
{: windows}

또는 푸시 API 메소드에 액세스해야 하는 각 인스턴스에 대해 `MFPPush.GetInstance().methodName()`을 호출할 수 있습니다.
{: windows}

##### 인증 확인 핸들러
{: #challenge-handlers }
{: windows}

`push.mobileclient` 범위가 **보안 검사**에 맵핑된 경우 푸시 API를 사용하기 전에 일치하는 **인증 확인 핸들러**가 존재하며 등록되어 있는지 확인해야 합니다.
{: windows}

[인증 정보 유효성 검증 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/credentials-validation/windows-8-10/) 튜토리얼에서 인증 확인 핸들러에 대해 자세히 알아보십시오.
{: note}
{: windows}

#### 클라이언트 측
{: #client-side }
{: windows}

| C# 메소드                                                                                                | 설명                                                             |
|--------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------|
| [`Initialize()`](#initialization) | 제공된 컨텍스트에 대한 MFPPush를 초기화합니다. |
| [`IsPushSupported()`](#is-push-supported) | 디바이스가 푸시 알림을 지원하는지 확인합니다. |
| [`RegisterDevice(JObject options)`](#register-device--send-device-token) | 디바이스를 푸시 알림 서비스에 등록합니다. |
| [`GetTags()`](#get-tags) | 푸시 알림 서비스 인스턴스에서 사용 가능한 태그를 검색합니다. |
| [`Subscribe(String[] Tags)`](#subscribe) | 디바이스가 지정된 태그를 구독하도록 합니다. |
| [`GetSubscriptions()`](#get-subscriptions) | 현재 디바이스가 구독하는 모든 태그를 검색합니다. |
| [`Unsubscribe(String[] Tags)`](#unsubscribe) | 특정 태그의 구독을 해지합니다. |
| [`UnregisterDevice()`](#unregister) | 푸시 알림 서비스에서 디바이스의 등록을 취소합니다. |
{: windows}

##### 초기화
{: #initialization }
{: windows}

클라이언트 애플리케이션이 MFPPush 서비스에 연결하려면 초기화가 필요합니다.
{: windows}

* 다른 MFPPush API를 사용하기 전에 먼저 `Initialize` 메소드를 호출해야 합니다.
* 이는 수신된 푸시 알림을 처리하도록 콜백 함수를 등록합니다.
{: windows}

```csharp
MFPPush.GetInstance().Initialize();
```
{: codeblock}
{: windows}

##### 푸시가 지원되는지 여부
{: #is-push-supported }
{: windows}

디바이스가 푸시 알림을 지원하는지 확인합니다.
{: windows}

```csharp
Boolean isSupported = MFPPush.GetInstance().IsPushSupported();

if (isSupported ) {
    // Push is supported
} else {
    // Push is not supported
}
```
{: codeblock}
{: windows}

##### 디바이스 등록 및 디바이스 토큰 전송
{: #register-device--send-device-token }
{: windows}

디바이스를 푸시 알림 서비스에 등록합니다.
{: windows}

```csharp
JObject Options = new JObject();
MFPPushMessageResponse Response = await MFPPush.GetInstance().RegisterDevice(Options);         
if (Response.Success == true)
{
    // Successfully registered
} else {
    // Registration failed with error
}
```
{: codeblock}
{: windows}

##### 태그 가져오기
{: #get-tags }
{: windows}

푸시 알림 서비스에서 사용 가능한 모든 태그를 검색합니다.
{: windows}

```csharp
MFPPushMessageResponse Response = await MFPPush.GetInstance().GetTags();
if (Response.Success == true)
{
    Message = new MessageDialog("Avalibale Tags: " + Response.ResponseJSON["tagNames"]);
} else{
    Message = new MessageDialog("Failed to get Tags list");
}
```
{: codeblock}
{: windows}

##### 구독
{: #subscribe }
{: windows}

원하는 태그를 구독합니다.
{: windows}

```csharp
string[] Tags = ["Tag1" , "Tag2"];

// Get subscription tag
MFPPushMessageResponse Response = await MFPPush.GetInstance().Subscribe(Tags);
if (Response.Success == true)
{
    //successfully subscribed to push tag
}
else
{
    //failed to subscribe to push tags
}
```
{: codeblock}
{: windows}

##### 구독 가져오기
{: #get-subscriptions }
{: windows}

현재 디바이스가 구독하는 태그를 검색합니다.
{: windows}

```csharp
MFPPushMessageResponse Response = await MFPPush.GetInstance().GetSubscriptions();
if (Response.Success == true)
{
    Message = new MessageDialog("Avalibale Tags: " + Response.ResponseJSON["tagNames"]);
}
else
{
    Message = new MessageDialog("Failed to get subcription list...");
}
```
{: codeblock}
{: windows}

##### 구독 해지
{: #unsubscribe }
{: windows}

태그의 구독을 해지합니다.
{: windows}

```csharp
string[] Tags = ["Tag1" , "Tag2"];

// unsubscribe tag
MFPPushMessageResponse Response = await MFPPush.GetInstance().Unsubscribe(Tags);
if (Response.Success == true)
{
    //succes
}
else
{
    //failed to subscribe to tags
}
```
{: codeblock}
{: windows}

##### 등록 취소
{: #unregister }
{: windows}

푸시 알림 서비스 인스턴스에서 디바이스의 등록을 취소합니다.
{: windows}

```csharp
MFPPushMessageResponse Response = await MFPPush.GetInstance().UnregisterDevice();         
if (Response.Success == true)
{
    // Successfully registered
} else {
    // Registration failed with error
}
```
{: codeblock}
{: windows}

#### 푸시 알림 처리
{: #handling-a-push-notification }
{: windows}

푸시 알림을 처리하려면 `MFPPushNotificationListener`를 설정해야 합니다. 이는 다음 메소드를 구현하여 수행할 수 있습니다.
{: windows}

1. MFPPushNotificationListener 유형의 인터페이스를 사용하여 클래스를 작성하십시오.

    ```csharp
    internal class NotificationListner : MFPPushNotificationListener
    {
         public async void onReceive(String properties, String payload)
    {
         // Handle push notification here      
    }
    }
    ```
    {: codeblock}
{: windows}
2. `MFPPush.GetInstance().listen(new NotificationListner())`를 호출하여 클래스를 리스너로 설정하십시오.
3. onReceive 메소드에서 푸시 알림을 수신하고 원하는 동작에 대한 알림을 처리할 수 있습니다.
{: windows}

#### Windows Universal Push Notifications Service
{: #windows-universal-push-notifications-service }
{: windows}

서버 구성에서 특정 포트를 열지 않아도 됩니다.
{: windows}

WNS는 일반 http 또는 https 요청을 사용합니다.
{: windows}
