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


# クライアント・アプリケーションでのプッシュ通知の処理
{: #handling_push_notifications_in_client_applications}

iOS、Android、および Windows のネイティブ・ベースまたは Cordova ベースのアプリケーションで着信プッシュ通知を受け取り、プッシュ通知を表示できるようにするには、最初にアプリケーションをセットアップし、API を実装しておく必要があります。
{: shortdesc}

クライアント・アプリケーションで着信プッシュ通知を処理する方法について理解するには、以下のセクションを参照してください。

### Android でのプッシュ通知の処理
{: #handling_push_notifications_in_android }
{: android}
受け取ったプッシュ通知を Android アプリケーションが処理できるようにするためには、Google Play Services のサポートを構成する必要があります。 アプリケーションが構成されると、{{ site.data.keyword.mobilefirst_notm }} が提供する通知 API を使用して、デバイスの登録や登録抹消、タグへのサブスクライブやアンサブスクライブを実行できます。 このチュートリアルでは、Android アプリケーションでプッシュ通知を処理する方法について学習します。
{: android}

**前提条件**
{: android}
* ローカルで稼働している {{ site.data.keyword.mfserver_short_notm }}、またはリモートで稼働している {{ site.data.keyword.mfserver_short_notm }}
* 開発者ワークステーションに {{ site.data.keyword.mobilefirst_notm  }} CLI がインストールされている
{: android}

#### 通知構成
{: #notifications-configuration }
{: android}

新しい Android Studio プロジェクトを作成するか、または既存のプロジェクトを使用します。  
{{ site.data.keyword.mobilefirst_notm }} Native Android SDK がプロジェクトにまだ存在しない場合は、[Android アプリケーションへの {{ site.data.keyword.mobilefoundation_short }} SDK の追加](https://cloud.ibm.com/docs/services/mobilefoundation/add_sdk_to_app.html#add_sdk_to_app)チュートリアルの説明に従ってください。
{: android}

##### プロジェクトのセットアップ
{: #project-setup }
{: android}

1. **Android →「Gradle Scripts」**で、**build.gradle (Module: app)** ファイルを選択し、以下の行を `dependencies` に追加します。
{: android}

    ```bash
    com.google.android.gms:play-services-gcm:9.0.2
    ```
    {: codeblock}
    {: android}

    [Google の既知の問題](https://code.google.com/p/android/issues/detail?id=212879)のために、Play Services の最新バージョン (現行は 9.2.0) は使用できません。 下位バージョンを使用してください。
    {: note}
    {: android}

    以下も追加します。
    ```xml
    compile group: 'com.ibm.mobile.foundation',
             name: 'ibmmobilefirstplatformfoundationpush',
             version: '8.0.+',
             ext: 'aar',
             transitive: true
    ```
    {: codeblock}
    {: android}

    または、以下のように 1 行にします。
    ```xml
    compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundationpush:8.0.+'
    ```
    {: codeblock}
    {: android}

1. **Android →「app」→「manifests」**で、`AndroidManifest.xml` ファイルを開きます。
    * 以下の許可を `manifest` タグの上部に追加します。

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

    * 以下を `application` タグに追加します。

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

      `your.application.package.name` の部分は、必ずご使用のアプリケーションの実際のパッケージ名に置き換えてください。
      {: note}
      {: android}

    * 以下の `intent-filter` をアプリケーションのアクティビティーに追加します。
        ```xml
        <intent-filter>
            <action android:name="your.application.package.name.IBMPushNotification" />
            <category android:name="android.intent.category.DEFAULT" />
        </intent-filter>
        ```
        {: codeblock}
        {: android}

#### 通知 API
{: #notifications-api }
{: android}

##### MFPPush インスタンス
{: #mfppush-instance }
{: android}

すべての API 呼び出しは、`MFPPush` のインスタンスから呼び出される必要があります。  これを行うには、クラス・レベルのフィールド (`private MFPPush push = MFPPush.getInstance();` など) を作成し、その後、クラス内で一貫して `push.<api-call>` を呼び出します。
{: android}

代わりに、プッシュ API メソッドにアクセスする必要があるインスタンスごとに `MFPPush.getInstance().<api_call>` を呼び出すこともできます。
{: android}

##### チャレンジ・ハンドラー
{: #challenge-handlers }
{: android}

`push.mobileclient` スコープが**セキュリティー検査**にマップされる場合、プッシュ API を使用する前に、一致する**チャレンジ・ハンドラー**が存在し、登録済みであることを確認する必要があります。
{: android}

チャレンジ・ハンドラーについて詳しくは、[Credentials Validation ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/credentials-validation/android/) のチュートリアルを参照してください。
{: note}
{: android}

##### クライアント・サイド
{: #client-side }
{: android}

| Java メソッド | 説明 |
|-----------------------------------------------------------------------------------|-------------------------------------------------------------------------|
| [`initialize(Context context);`](#initialization) | 提供されたコンテキストの MFPPush を初期化します。 |
| [`isPushSupported();`](#is-push-supported) | デバイスがプッシュ通知をサポートするかどうか。 |
| [`registerDevice(JSONObject, MFPPushResponseListener);`](#register-device) | デバイスをプッシュ通知サービスに登録します。 |
| [`getTags(MFPPushResponseListener)`](#get-tags) | プッシュ通知サービス・インスタンス内で使用可能なタグを取得します。 |
| [`subscribe(String[] tagNames, MFPPushResponseListener)`](#subscribe) | 指定されたタグにデバイスをサブスクライブします。 |
| [`getSubscriptions(MFPPushResponseListener)`](#get-subscriptions) | デバイスが現在サブスクライブしているタグをすべて取得します。 |
| [`unsubscribe(String[] tagNames, MFPPushResponseListener)`](#unsubscribe) | 特定のタグからアンサブスクライブします。 |
| [`unregisterDevice(MFPPushResponseListener)`](#unregister) | プッシュ通知サービスからデバイスを登録抹消します。 |
{: android}

###### 初期化
{: #initialization }
{: android}

クライアント・アプリケーションが、正しいアプリケーション・コンテキストの MFPPush サービスに接続するために必要です。
{: android}

* 最初に API メソッドを呼び出してから、その他の MFPPush API を使用する必要があります。
* 受け取ったプッシュ通知を処理するコールバック関数を登録します。
{: android}

```java
MFPPush.getInstance().initialize(this);
```
{: codeblock}
{: android}

###### プッシュがサポートされるか
{: #is-push-supported }
{: android}

デバイスがプッシュ通知をサポートするかどうかをチェックします。
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

###### デバイスの登録
{: #register-device }
{: android}

デバイスをプッシュ通知サービスに登録します。
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

###### タグの取得
{: #get-tags }
{: android}

プッシュ通知サービスからすべての使用可能なタグを取得します。
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

###### サブスクライブ
{: #subscribe }
{: android}

目的のタグにサブスクライブします。
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

###### サブスクリプションの取得
{: #get-subscriptions }
{: android}

デバイスが現在サブスクライブしているタグを取得します。
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

###### アンサブスクライブ
{: #unsubscribe }
{: android}

タグからアンサブスクライブします。
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

###### 登録抹消
{: #unregister }
{: android}

プッシュ通知サービス・インスタンスからデバイスを登録抹消します。
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

#### プッシュ通知の処理
{: #handling-a-push-notification }
{: android}

プッシュ通知を処理するためには、`MFPPushNotificationListener` をセットアップする必要があります。  これは、以下のいずれかのメソッドを実装することで実現できます。
{: android}

##### オプション 1
{: #option-one }
{: android}

プッシュ通知を処理する必要があるアクティビティー内で、以下のようにします。
{: android}

1. `implements MFPPushNofiticationListener` をクラス宣言に追加します。
2. `onCreate` メソッド内で `MFPPush.getInstance().listen(this)` を呼び出すことで、このクラスがリスナーになるように設定します。
2. 次に、以下の*必須* メソッドを追加する必要があります。
    ```java
    @Override
    public void onReceive(MFPSimplePushNotification mfpSimplePushNotification) {
         // Handle push notification here
    }
    ```
    {: codeblock}
    {: android}

3. このメソッド内で `MFPSimplePushNotification` を受け取り、目的の動作にあわせて通知を処理できます。
{: android}

##### オプション 2
{: #option-two }
{: android}

下記の概略のように、`MFPPush` のインスタンスで `listen(new MFPPushNofiticationListener())` を呼び出すことで、リスナーを作成します。
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

#### Android 上のクライアント・アプリケーションの FCM へのマイグレーション
{: #migrate-to-fcm }
{: android}

Google Cloud Messaging (GCM) は[非推奨](https://developers.google.com/cloud-messaging/faq)となっており、Firebase Cloud Messaging (FCM) と統合されています。 Google は、2019 年 4 月までにほとんどの GCM サービスをオフにします。
{: android}

GCM プロジェクトを使用する場合は、[Android 上の GCM クライアント・アプリケーションを FCM にマイグレーション](https://developers.google.com/cloud-messaging/android/android-migrate-fcm)してください。
{: android}

現時点では、GCM サービスを使用する既存のアプリケーションは引き続きそのまま機能します。 プッシュ通知サービスは FCM エンドポイントを使用するように更新されているため、将来はすべての新しいアプリケーションが FCM を使用する必要があります。
{: android}

FCM へのマイグレーション後に、古い GCM 資格情報ではなく FCM 資格情報を使用するようにプロジェクトを更新してください。
{: note}
{: android}

##### FCM プロジェクトのセットアップ
{: #fcm_project_setup}
{: android}

FCM でのアプリケーションのセットアップは、古い GCM モデルと比較すると少し異なります。
{: android}

1. 通知プロバイダーの資格情報を取得して、FCM プロジェクトを作成し、同じものを Android アプリケーションに追加します。 アプリケーションのパッケージ名を `com.ibm.mobilefirstplatform.clientsdk.android.push` として含めます。 `google-services.json` ファイルの生成を終了したステップまでは、[ここにある資料](https://cloud.ibm.com/docs/services/mobilepush/push_step_1.html#push_step_1_android)を参照してください。
   {: android}
2. Gradle ファイルを構成します。 アプリケーションの `build.gradle` ファイルに以下のものを追加します。
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

    - ルートの build.gradle の `buildscript` セクションに以下の依存関係を追加します
      `classpath 'com.google.gms:google-services:3.0.0'`
      {: codeblock}
      {: android}

    - 以下の GCM プラグインを build.gradle ファイルから除去します `compile  com.google.android.gms:play-services-gcm:+`
     {: android}
 3. AndroidManifest ファイルを構成します。 `AndroidManifest.xml` で以下の変更が必要です。
    {: android}

    **以下の項目を削除します。**
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

    **以下の項目を変更する必要があります。**
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

    **これらの項目を以下のように変更します。**
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

    **以下の項目を追加します。**
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

 4. Android Studio でアプリケーションを開きます。 **ステップ 1** で作成した `google-services.json` ファイルをアプリケーション・ディレクトリー内にコピーします。 `google-service.json` ファイルには、追加したパッケージ名が含まれていることに注意してください。		
 5. SDK をコンパイルします。 アプリケーションをビルドします。
{: android}

### iOS でのプッシュ通知の処理
{: #handling_push_notifications_in_ios }
{: ios}

{{ site.data.keyword.mobilefirst_notm }} が提供する通知 API を使用して、デバイスの登録や登録抹消、タグへのサブスクライブやアンサブスクライブを実行できます。 このチュートリアルでは、Swift を使用して iOS アプリケーションでプッシュ通知を処理する方法について学習します。
{: shortdesc}
{: ios}

サイレント通知または対話式通知については、以下を参照してください。

* [サイレント通知](/docs/services/mobilefoundation?topic=mobilefoundation-silent_notifications#silent_notifications)
* [対話式通知](/docs/services/mobilefoundation?topic=mobilefoundation-interactive_notifications#interactive_notifications)
{: ios}

**前提条件**
{: ios}

* ローカルで稼働している {{ site.data.keyword.mfserver_short }}、またはリモートで稼働している {{ site.data.keyword.mfserver_short }}
* 開発者ワークステーションに {{ site.data.keyword.mfp_cli_long_notm }} がインストールされていること
{: ios}

#### 通知構成
{: #notifications-configuration }
{: ios}

新しい Xcode プロジェクトを作成するか、または既存のプロジェクトを使用します。
{{ site.data.keyword.mobilefirst_notm }} Native iOS SDK がプロジェクトにまだ存在しない場合は、[iOS  アプリケーションへの {{ site.data.keyword.mobilefoundation_short }} SDK の追加](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/ios/)チュートリアルの説明に従ってください。
{: ios}

#### プッシュ SDK の追加
{: #adding-the-push-sdk }
{: ios}

1. プロジェクトの既存の **podfile** を開き、以下の行を追加します。
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

    - 使用する Xcode プロジェクトのターゲットの名前で **Xcode-project-target** を置き換えてください。
2. **podfile** を保存して閉じます。
3. **コマンド・ライン**・ウィンドウからプロジェクトのルート・フォルダーにナビゲートします。
4. コマンド `pod install` を実行します。
5. **.xcworkspace** ファイルを使用して、プロジェクトを開きます。
{: ios}

#### 通知 API
{: #notifications-api }
{: ios}

##### MFPPush インスタンス
{: #mfppush-instance }
{: ios}

すべての API 呼び出しは、`MFPPush` のインスタンスから呼び出される必要があります。 これを行うには、ビュー・コントローラー内で `var` を使用し (`var push = MFPPush.sharedInstance();` など)、その後、ビュー・コントローラー内で一貫して `push.methodName()` を呼び出します。
{: ios}

代わりに、プッシュ API メソッドにアクセスする必要があるインスタンスごとに `MFPPush.sharedInstance().methodName()` を呼び出すこともできます。
{: ios}

#### チャレンジ・ハンドラー
{: #challenge-handlers }
{: ios}

`push.mobileclient` スコープが**セキュリティー検査**にマップされる場合、プッシュ API を使用する前に、一致する**チャレンジ・ハンドラー**が存在し、登録済みであることを確認する必要があります。
{: ios}

チャレンジ・ハンドラーについて詳しくは、[Credentials Validation ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/credentials-validation/ios/) のチュートリアルを参照してください。
{: note}
{: ios}

#### クライアント・サイド
{: #client-side }
{: ios}

| Swift メソッド | 説明  |
|---------------|--------------|
| [`initialize()`](#initialization) | 提供されたコンテキストの MFPPush を初期化します。 |
| [`isPushSupported()`](#is-push-supported) | デバイスがプッシュ通知をサポートするかどうか。 |
| [`registerDevice(completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#register-device--send-device-token) | デバイスをプッシュ通知サービスに登録します。|
| [`sendDeviceToken(deviceToken: NSData!)`](#register-device--send-device-token) | デバイス・トークンをサーバーに送信します。 |
| [`getTags(completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#get-tags) | プッシュ通知サービス・インスタンス内で使用可能なタグを取得します。 |
| [`subscribe(tagsArray: [AnyObject], completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#subscribe) | 指定されたタグにデバイスをサブスクライブします。 |
| [`getSubscriptions(completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#get-subscriptions)  | デバイスが現在サブスクライブしているタグをすべて取得します。 |
| [`unsubscribe(tagsArray: [AnyObject], completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#unsubscribe) | 特定のタグからアンサブスクライブします。 |
| [`unregisterDevice(completionHandler: ((WLResponse!, NSError!) -> Void)!)`](#unregister) | プッシュ通知サービスからデバイスを登録抹消します。              |
{: ios}

##### 初期化
{: #initialization }
{: ios}

初期化は、クライアント・アプリケーションが MFPPush サービスに接続するために必要です。
{: ios}

* 最初に `initialize` メソッドを呼び出してから、その他の MFPPush API を使用する必要があります。
* このメソッドは、受け取ったプッシュ通知を処理するコールバック関数を登録します。
{: ios}

```swift
MFPPush.sharedInstance().initialize();
```
{: codeblock}
{: ios}

##### プッシュがサポートされるか
{: #is-push-supported }
{: ios}

デバイスがプッシュ通知をサポートするかどうかをチェックします。
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

##### デバイスの登録 &amp; デバイス・トークンの送信
{: #register-device--send-device-token }
{: ios}

デバイスをプッシュ通知サービスに登録します。
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

これは一般的に `didRegisterForRemoteNotificationsWithDeviceToken` メソッドの **AppDelegate** 内で呼び出されます。
{: note}
{: ios}

##### タグの取得
{: #get-tags }
{: ios}

プッシュ通知サービスからすべての使用可能なタグを取得します。
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

##### サブスクライブ
{: #subscribe }
{: ios}

目的のタグにサブスクライブします。
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

##### サブスクリプションの取得
{: #get-subscriptions }
{: ios}

デバイスが現在サブスクライブしているタグを取得します。
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

##### アンサブスクライブ
{: #unsubscribe }
{: ios}

タグからアンサブスクライブします。
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

##### 登録抹消
{: #unregister }
{: ios}

プッシュ通知サービス・インスタンスからデバイスを登録抹消します。
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

#### プッシュ通知の処理
{: #handling-a-push-notification }
{: ios}

プッシュ通知は、ネイティブ iOS フレームワークによって直接的に処理されます。 アプリケーション・ライフサイクルに応じて、いろいろなメソッドが iOS フレームワークによって呼び出されます。
{: ios}

例えば、アプリケーションの実行中に単純な通知を受け取った場合は、**AppDelegate** の `didReceiveRemoteNotification` がトリガーされます。
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

iOS での通知の処理について詳しくは、[Apple の資料 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html#//apple_ref/doc/uid/TP40008194-CH8-SW1) を参照してください。
{: note}
{: ios}

### Cordova でのプッシュ通知の処理
{: #handling_push_notifications_in_cordova }
{: cordova}

iOS、Android、および Windows の Cordova アプリケーションでプッシュ通知を受け取り、プッシュ通知を表示できるようにするには、**cordova-plugin-mfp-push** Cordova プラグインを Cordova プロジェクトに追加する必要があります。 アプリケーションが構成されると、{{ site.data.keyword.mobilefirst_notm }} が提供する通知 API を使用して、デバイスの登録や登録抹消、タグへのサブスクライブやアンサブスクライブ、および通知の処理を実行できます。 このチュートリアルでは、Cordova アプリケーションでプッシュ通知を処理する方法について学習します。
{: shortdesc}
{: cordova}

ある問題のために、認証済み通知は、現在 Cordova アプリケーションでは**サポートされていません**。 しかし、予備手段が用意されており、各 `MFPPush` API 呼び出しを `WLAuthorizationManager.obtainAccessToken("push.mobileclient").then( ... );` でラップできます。
{: note}
{: cordova}

iOS でのサイレント通知または対話式通知については、以下を参照してください。
{: cordova}

* [サイレント通知](/docs/services/mobilefoundation?topic=mobilefoundation-silent_notifications#silent_notifications)
* [対話式通知](/docs/services/mobilefoundation?topic=mobilefoundation-interactive_notifications#interactive_notifications)
{: cordova}

**前提条件**
{: cordova}

* ローカルで稼働している {{ site.data.keyword.mfserver_short }}、またはリモートで稼働している {{ site.data.keyword.mfserver_short }}
* 開発者ワークステーションに {{ site.data.keyword.mfp_cli_long_notm }} がインストールされていること
* 開発者ワークステーションに Cordova CLI がインストールされていること
{: cordova}

#### 通知構成
{: #notifications-configuration }
{: cordova}

新しい Cordova プロジェクトを作成するか既存のプロジェクトを使用し、サポートされるプラットフォーム (iOS、Android、Windows) を 1 つ以上追加します。
{: cordova}

{{ site.data.keyword.mobilefirst_notm }} Cordova SDK がプロジェクトにまだ存在しない場合は、[Cordova アプリケーションへの {{ site.data.keyword.mobilefirst_notm }} SDK の追加](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/cordova/)チュートリアルの説明に従ってください。
{: cordova}
{: note}

#### プッシュ・プラグインの追加
{: #adding-the-push-plug-in }
{: cordova}

1. **コマンド・ライン**・ウィンドウから Cordova プロジェクトのルートにナビゲートします。  
2. 以下のコマンドを実行して、プッシュ・プラグインを追加します。
    ```bash
    cordova plugin add cordova-plugin-mfp-push
    ```
    {: codeblock}

3. 以下のコマンドを実行して、Cordova プロジェクトをビルドします。
    ```bash
    cordova build
    ```
    {: codeblock}
{: cordova}

#### iOS プラットフォーム
{: #ios-platform }
{: cordova}

iOS プラットフォームでは追加のステップが必要です。  
{: cordova}

Xcode で、**「Capabilities」**画面を使用してアプリケーションのプッシュ通知を有効にします。
{: cordova}

アプリケーションに対して選択する bundleId は、先に Apple Developer サイトで作成した AppId に一致しなければなりません。
{: important}
{: cordova}

![Xcode 内においてこの機能が存在する場所を示すイメージ](images/push-capability.png)
{: cordova}

#### Android プラットフォーム
{: #android-platform }
{: cordova}

Android プラットフォームでは追加のステップが必要です。  
{: cordova}

Android Studio では、以下の `activity` を `application` タグに追加します。
```xml
<activity android:name="com.ibm.mobilefirstplatform.clientsdk.android.push.api.MFPPushNotificationHandler" android:theme="@android:style/Theme.NoDisplay"/>
```
{: codeblock}
{: cordova}

#### 通知 API
{: #notifications-api }
{: cordova}

##### クライアント・サイド
{: #client-side }
{: cordova}

| Javascript 関数 | 説明 |
| --- | --- |
| [`MFPPush.initialize(success, failure)`](#initialization) | MFPPush インスタンスを初期化します。 |
| [`MFPPush.isPushSupported(success, failure)`](#is-push-supported) | デバイスがプッシュ通知をサポートするかどうか。 |
| [`MFPPush.registerDevice(options, success, failure)`](#register-device) | デバイスをプッシュ通知サービスに登録します。 |
| [`MFPPush.getTags(success, failure)`](#get-tags) | プッシュ通知サービス・インスタンス内で使用可能なすべてのタグを取得します。 |
| [`MFPPush.subscribe(tag, success, failure)`](#subscribe) | 特定のタグにサブスクライブします。 |
| [`MFPPush.getSubsciptions(success, failure)`](#get-subscriptions) | デバイスが現在サブスクライブしているタグを取得します。 |
| [`MFPPush.unsubscribe(tag, success, failure)`](#unsubscribe) | 特定のタグからアンサブスクライブします。 |
| [`MFPPush.unregisterDevice(success, failure)`](#unregister) | プッシュ通知サービスからデバイスを登録抹消します。 |
{: cordova}

##### API 実装
{: #api-implementation }
{: cordova}

###### 初期化
{: #initialization }
{: cordova}

**MFPPush** インスタンスを初期化します。
{: cordova}

- クライアント・アプリケーションが、正しいアプリケーション・コンテキストの MFPPush サービスに接続するために必要です。  
- 最初に API メソッドを呼び出してから、その他の MFPPush API を使用する必要があります。
- 受け取ったプッシュ通知を処理するコールバック関数を登録します。
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

###### プッシュがサポートされるか
{: #is-push-supported }
{: cordova}

デバイスがプッシュ通知をサポートするかどうかをチェックします。
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

###### デバイスの登録
{: #register-device }
{: cordova}

デバイスをプッシュ通知サービスに登録します。 必要なオプションがない場合、オプションは `null` に設定できます。
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

###### タグの取得
{: #get-tags }
{: cordova}

プッシュ通知サービスからすべての使用可能なタグを取得します。
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

###### サブスクライブ
{: #subscribe }
{: cordova}

目的のタグにサブスクライブします。
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

###### サブスクリプションの取得
{: #get-subscriptions }
{: cordova}

デバイスが現在サブスクライブしているタグを取得します。
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

###### アンサブスクライブ
{: #unsubscribe }
{: cordova}

タグからアンサブスクライブします。
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

###### 登録抹消
{: #unregister }
{: cordova}

プッシュ通知サービス・インスタンスからデバイスを登録抹消します。
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

#### プッシュ通知の処理
{: #handling-a-push-notification }
{: cordova}

登録済みのコールバック関数内で応答オブジェクトを操作することで、受け取ったプッシュ通知を処理できます。
{: cordova}

```javascript
var notificationReceived = function(message) {
    alert(JSON.stringify(message));
};
```
{: codeblock}
{: cordova}

### Windows 8.1 Universal および Windows 10 UWP でのプッシュ通知の処理
{: #handling_push_notifications_in_windows }
{: windows}

{{ site.data.keyword.mobilefirst_notm }} が提供する通知 API を使用して、デバイスの登録や登録抹消、タグへのサブスクライブやアンサブスクライブを実行できます。 このチュートリアルでは、C# を使用して、ネイティブの Windows 8.1 Universal アプリケーションおよび Windows 10 UWP アプリケーションでプッシュ通知を処理する方法について学習します。
{: windows}

**前提条件**
{: windows}

* ローカルで稼働している {{ site.data.keyword.mfserver_short_notm }}、またはリモートで稼働している {{ site.data.keyword.mfserver_short_notm }}
* 開発者ワークステーションに {{ site.data.keyword.mobilefirst_notm  }} CLI がインストールされている
{: windows}

#### 通知構成
{: #notifications-configuration }
{: windows}

新しい Visual Studio プロジェクトを作成するか、または既存のプロジェクトを使用します。  
{: windows}

{{ site.data.keyword.mobilefirst_notm }} Native Windows SDK がプロジェクトにまだ存在しない場合は、[Windows アプリケーションへの {{ site.data.keyword.mobilefirst_notm }} SDK の追加](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/sdk/windows-8-10/)チュートリアルの説明に従ってください。
{: windows}

#### プッシュ SDK の追加
{: #adding-the-push-sdk }
{: windows}

1. 「ツール」→「NuGet パッケージ マネージャー」→「パッケージ マネージャー コンソール」を選択します。
2. {{ site.data.keyword.mobilefirst_notm }} プッシュ・コンポーネントをインストールするプロジェクトを選択します。
3. **Install-Package IBM.MobileFirstPlatformFoundationPush** コマンドを実行して、{{ site.data.keyword.mobilefirst_notm }} プッシュ SDK を追加します。
{: windows}

#### WNS 構成の前提条件
{: pre-requisite-wns-configuration }
{: windows}

1. アプリケーションにトースト通知機能が備わっていることを確認します。 これは Package.appxmanifest 内で有効にできます。
2. `Package Identity Name` と `Publisher` が、WNS に登録されている値で更新されている必要があります。
3. (オプション) TemporaryKey.pfx ファイルを削除します。
{: windows}

#### 通知 API
{: #notifications-api }
{: windows}

##### MFPPush インスタンス
{: #mfppush-instance }
{: windows}

すべての API 呼び出しは、`MFPPush` のインスタンスから呼び出される必要があります。  これを行うには、変数 (`private MFPPush PushClient = MFPPush.GetInstance();` など) を作成し、その後、クラス内で一貫して `PushClient.methodName()` を呼び出します。
{: windows}

代わりに、プッシュ API メソッドにアクセスする必要があるインスタンスごとに `MFPPush.GetInstance().methodName()` を呼び出すこともできます。
{: windows}

##### チャレンジ・ハンドラー
{: #challenge-handlers }
{: windows}

`push.mobileclient` スコープが**セキュリティー検査**にマップされる場合、プッシュ API を使用する前に、一致する**チャレンジ・ハンドラー**が存在し、登録済みであることを確認する必要があります。
{: windows}

チャレンジ・ハンドラーについて詳しくは、[Credentials Validation ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/authentication-and-security/credentials-validation/windows-8-10/) のチュートリアルを参照してください。
{: note}
{: windows}

#### クライアント・サイド
{: #client-side }
{: windows}

| C Sharp メソッド                                                                                                | 説明                                                             |
|--------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------|
| [`Initialize()`](#initialization)                                                                            | 提供されたコンテキストの MFPPush を初期化します。                               |
| [`IsPushSupported()`](#is-push-supported)                                                                    | デバイスがプッシュ通知をサポートするかどうか。                             |
| [`RegisterDevice(JObject options)`](#register-device--send-device-token)                  | デバイスをプッシュ通知サービスに登録します。               |
| [`GetTags()`](#get-tags)                                | プッシュ通知サービス・インスタンス内で使用可能なタグを取得します。 |
| [`Subscribe(String[] Tags)`](#subscribe)     | 指定されたタグにデバイスをサブスクライブします。                          |
| [`GetSubscriptions()`](#get-subscriptions)              | デバイスが現在サブスクライブしているタグをすべて取得します。               |
| [`Unsubscribe(String[] Tags)`](#unsubscribe) | 特定のタグからアンサブスクライブします。                                  |
| [`UnregisterDevice()`](#unregister)                     | プッシュ通知サービスからデバイスを登録抹消します。              |
{: windows}

##### 初期化
{: #initialization }
{: windows}

初期化は、クライアント・アプリケーションが MFPPush サービスに接続するために必要です。
{: windows}

* 最初に `Initialize` メソッドを呼び出してから、その他の MFPPush API を使用する必要があります。
* このメソッドは、受け取ったプッシュ通知を処理するコールバック関数を登録します。
{: windows}

```csharp
MFPPush.GetInstance().Initialize();
```
{: codeblock}
{: windows}

##### プッシュがサポートされるか
{: #is-push-supported }
{: windows}

デバイスがプッシュ通知をサポートするかどうかをチェックします。
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

##### デバイスの登録 &amp; デバイス・トークンの送信
{: #register-device--send-device-token }
{: windows}

デバイスをプッシュ通知サービスに登録します。
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

##### タグの取得
{: #get-tags }
{: windows}

プッシュ通知サービスからすべての使用可能なタグを取得します。
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

##### サブスクライブ
{: #subscribe }
{: windows}

目的のタグにサブスクライブします。
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

##### サブスクリプションの取得
{: #get-subscriptions }
{: windows}

デバイスが現在サブスクライブしているタグを取得します。
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

##### アンサブスクライブ
{: #unsubscribe }
{: windows}

タグからアンサブスクライブします。
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

##### 登録抹消
{: #unregister }
{: windows}

プッシュ通知サービス・インスタンスからデバイスを登録抹消します。
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

#### プッシュ通知の処理
{: #handling-a-push-notification }
{: windows}

プッシュ通知を処理するためには、`MFPPushNotificationListener` をセットアップする必要があります。  これは、以下のメソッドを実装することで実現できます。
{: windows}

1. MFPPushNotificationListener タイプのインターフェースを使用してクラスを作成します。

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
2. `MFPPush.GetInstance().listen(new NotificationListner())` を呼び出すことで、このクラスがリスナーになるように設定します。
3. onReceive メソッド内でプッシュ通知を受け取り、目的の動作にあわせて通知を処理できます。
{: windows}

#### Windows Universal Push Notifications Service
{: #windows-universal-push-notifications-service }
{: windows}

サーバー構成内で特定のポートをオープンする必要はありません。
{: windows}

WNS では通常の http 要求または https 要求が使用されます。
{: windows}
