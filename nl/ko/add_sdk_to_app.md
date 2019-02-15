---

copyright:
  years: 2018, 2019
lastupdated:  "2019-01-04"

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
{:reactnative: .ph data-hd-programlang='reactnative'}
{:csharp: .ph data-hd-programlang='csharp'}
{:ios: .ph data-hd-programlang='iOS'}
{:android: .ph data-hd-programlang='Android'}
{:cordova: .ph data-hd-programlang='Cordova'}

#	앱에 Mobile Foundation SDK 추가
{: #add_sdk_to_app}

### 앱에 Android SDK 추가
{: android}

Android Studio를 열고 Android 보기를 선택한 다음 **Gradle 스크립트**를 선택하고 `build.gradle(Module: app)` 파일을 선택한 다음 아래 단계를 따라 Android 애플리케이션에 Android SDK를 추가하십시오.
{: android}

1. `dependencies` 섹션에 다음 행을 추가하십시오.
   ```bash
   compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundation:8.0.+'
   ```
   {: codeblock}
   {: android}
2. `android` 섹션에 다음 행을 추가하십시오.
   ```
   packagingOptions {
              pickFirst 'META-INF/ASL2.0'
              pickFirst 'META-INF/LICENSE'
              pickFirst 'META-INF/NOTICE'
    }
  ```
  {: codeblock}
  {: android}
3. Android 보기에서 **app → manifests → AndroidManifest.xml** 파일을 여십시오. `application` 요소 위에 다음 권한을 추가하십시오.
   ```xml
   <uses-permission android:name="android.permission.INTERNET"/>
   <uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>
   ```
   {: codeblock}
   {: android}
4. 기존 `activity` 요소 옆에 MobileFirst UI 활동을 추가하십시오.
   ```xml
   <activity android:name="com.worklight.wlclient.ui.UIActivity" />
   ```
   {: codeblock}
   {: android}


### 앱에 iOS SDK 추가
{: ios}

Mobile Foundation 보안, 권한 부여, 로깅, 어댑터 호출 및 기타 코어 함수를 구현하는 API로 구성된 IBM Mobile Foundation용 코어 SDK입니다. 아래 단계를 따라 iOS 애플리케이션에 iOS SDK를 추가하십시오.
{: ios}

1. iOS 앱의 루트 폴더로 이동한 후 다음 명령을 실행하여 Podfile을 작성하십시오.
    ```bash
    pod init
    ```
    {: codeblock}
    {: ios}
2. 선호하는 코드 편집기를 사용하여 Podfile을 여십시오.
   ```bash
   use_frameworks!
   platform :ios, 8.0
   target "ProjectName" do
       pod 'IBMMobileFirstPlatformFoundation'
   end
   ```
   {: codeblock}
   {: ios}
3. 다음 명령을 사용하여 팟(Pod)을 업데이트하십시오.
   ```bash
   pod update
   ```
   {: codeblock}
   {: ios}
4. 다음 명령을 사용하여 팟(Pod)을 설치하십시오.
   ```bash
   pod install
   ```
   {: codeblock}
   {: ios}
5. [ProjectName].xcworkspace를 열어 Xcode에서 프로젝트를 여십시오.
6. `mfpclient.plist` 파일을 Xcode 작업공간에 추가하십시오.
7. 명령 프롬프트에서 프로젝트 폴더로 이동하여 Mobile Foundation 인스턴스에 앱을 등록하십시오.
   ```bash
mfpdev app register
   ```
   {: codeblock}
   {: ios}

### 앱에 Cordova SDK 추가
{: cordova}

Mobile Foundation 보안, 권한 부여, 로깅, 어댑터 호출 및 기타 코어 함수를 구현하는 API로 구성된 IBM Mobile Foundation용 코어 SDK입니다. 아래 단계를 따라 애플리케이션에 Cordova SDK를 추가하십시오.
{: cordova}

1. Cordova 프로젝트를 작성하십시오.
   ```bash
   cordova create Hello com.example.helloworld HelloWorld
   ```
   {: codeblock}
   {: cordova}
2. 디렉토리를 Cordova 프로젝트의 루트로 변경하십시오.
   ```bash
   cd Hello
   ```
   {: codeblock}
   {: cordova}
3. Cordova CLI를 사용하여 Cordova 프로젝트에 하나 이상의 지원되는 플랫폼을 추가하십시오.
   ```bash
   cordova platform add ios|android|windows
   ```
   {: codeblock}
   {: cordova}
4. Mobile Foundation Core Cordova 플러그인을 추가하십시오.
   ```bash
   cordova plugin add cordova-plugin-mfp
   ```
   {: codeblock}
   {: cordova}
5. 다음 명령을 실행하여 애플리케이션 리소스를 준비하십시오.
   ```bash
   cordova prepare
   ```
   {: codeblock}
   {: cordova}
6. Mobile Foundation Server에 애플리케이션을 등록하십시오.
   ```bash
mfpdev app register
   ```
   {: codeblock}
   {: cordova}

### 앱에 React Native SDK 플러그인 추가
{: reactnative}

기존 React Native 앱에 Mobile Foundation 기능을 추가하려면 `react-native-ibm-mobilefirst` 플러그인을 앱에 추가해야 합니다. `react-native-ibm-mobilefirst` 플러그인에는 Mobile Foundation SDK가 포함되어 있습니다. 아래 단계를 따라 React Native 애플리케이션에 React Native 플러그인을 추가하십시오.
{: reactnative}

1. 앱에 다른 `npm` 플러그인을 추가하는 방식과 동일하게 이 플러그인을 추가하십시오.
   ```bash
   npm install react-native-ibm-mobilefirst --save
   ```
   {: codeblock}
   {: reactnative}
2. 첫 번째 단계는 React Native 프로젝트(예: *MobileFirstApp*)를 작성하는 것입니다. React Native CLI를 사용하여 새 프로젝트를 작성하십시오.
   ```bash
   react-native init MobileFirstApp
   ```
   {: codeblock}
   {: reactnative}
3. 다음으로 react native 플러그인을 앱에 추가하십시오.
   ```bash
   cd MobileFirstApp
   npm install react-native-ibm-mobilefirst --save
   ```
   {: codeblock}
   {: reactnative}
4. 모든 기본 종속성이 React Native 프로젝트에 추가되도록 프로젝트를 링크하십시오.
   ```bash
   react-native link
   ```
   {: codeblock}
   {: reactnative}
5. Android의 경우 `AndroidManifest.xml`(`<PROJECT_ROOT>/android/app/src/main/`)을 다음과 같이 변경하십시오.
   ```xml
   <manifest 
          xmlns:android="http://schemas.android.com/apk/res/android" 
          xmlns:tools="http://schemas.android.com/tools"
          package="com.mobilefirstapp">
   ```
   {: codeblock}
   {: reactnative}
6. **tools:replace='android:allowBackup'**을 `application` 태그에 추가하십시오.
   ```xml
   <application
            android:name=".MainApplication"
            android:label="@string/app_name"
            android:icon="@mipmap/ic_launcher"
            android:allowBackup="false"
            android:theme="@style/AppTheme"
            tools:replace="android:allowBackup">
   ```
   {: codeblock}
   {: reactnative}
7. iOS의 경우 XCode를 열고 프로젝트 탐색기에서 `mfpclient.plist`를 `ios` 폴더에서 끌어서 놓으십시오. 이 단계는 iOS 플랫폼에만 적용됩니다.
{: reactnative}

