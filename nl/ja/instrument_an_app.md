---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: mobile analytics, instrumenting cordova app, instrumenting iOS app, instrumenting android app

subcollection:  mobilefoundation
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

# アプリのインスツルメント
{: #instrument_your_app}

Mobile Analytics は、{{ site.data.keyword.mobilefoundation_short }} サービスに組み込まれている機能です。 モバイル・アプリケーション開発者やアプリケーション所有者は、Mobile Analytics を使用して、主なアプリケーションの使用状況とパフォーマンスに関する洞察を得ることができます。

Mobile Analytics 機能を使用して、アプリケーションの使用状況とパフォーマンスをモニターし、その他の統計を取得するには、モバイル・アプリケーションをインスツルメント (計装) する必要があります。 アプリケーションをインスツルメントするには、以下の手順に従います。

1.  Mobile Analytics クライアント SDK をインポートしてインストールします。
2.  収集したい分析データのタイプに基づいてアプリケーションをインスツルメントします。

以下のセクションで、サポート対象プラットフォーム別に、この手順について詳しく説明します。

### Android アプリケーションのインスツルメント
{: #instrument_android_app}
{: android}
#### ステップ 1: Android 用の Mobile Analytics クライアント SDK をインポートしてインストールする
{: #install_analytics_sdk_android }
{: android}
[![Maven Central](https://maven-badges.herokuapp.com/maven-central/com.ibm.mobile.foundation/ibmmobilefirstplatformfoundation/badge.svg) ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://maven-badges.herokuapp.com/maven-central/com.ibm.mobile.foundation/ibmmobilefirstplatformfoundation)
[![Maven Central](https://maven-badges.herokuapp.com/maven-central/com.ibm.mobile.foundation/ibmmobilefirstplatformfoundationanalytics/badge.svg) ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://maven-badges.herokuapp.com/maven-central/com.ibm.mobile.foundation/ibmmobilefirstplatformfoundationanalytics)
{: android}

Mobile Analytics クライアント SDK には、Android プロジェクト用の依存関係マネージャーである Gradle が付属しています。 Gradle は、自動的に成果物をリポジトリーからダウンロードし、Android アプリケーションで使用できるようにします。
{: android}

1. [Android Studio ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](http://developer.android.com/sdk/index.html){: new_window} プロジェクトを作成するか、既存のプロジェクトを開きます。
{: android}

2. **アプリ・モジュール**の `build.gradle` ファイルを開きます。
{: android}

  Android プロジェクトには、プロジェクト用とアプリ・モジュール用に `build.gradle` ファイルが 2 つある場合があります。 必ず、**アプリ・モジュール**のファイルを使用してください。
  {: tip}
  {: android}

3. `build.gradle` ファイルの `Dependencies` セクションを探し、{{site.data.keyword.mobileanalytics_short}} クライアント SDK のコンパイルの依存関係を追加します。 リポジトリー・ステートメントは、以下のコード例のようにする必要があります。
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

  1 番目の依存関係は、Mobile Analytics クライアント SDK でアプリケーション・ランタイム・イベントをキャプチャーしてロギングするためのものです。2 番目の依存関係は、アプリケーション・ユーザーと対話するアプリ内ユーザー・フィードバックを有効化するためのものです。 この 2 番目の依存関係は、アプリ内ユーザー・フィードバックを有効にする場合にのみ必要です
  {: android}

4. **「ツール」&gt;「Android」&gt;「プロジェクトを Gradle ファイルと同期 (Sync Project with Gradle Files)」**の順にクリックしてプロジェクトを Gradle と同期します。
{: android}

5. Android プロジェクト用の `AndroidManifest.xml` ファイルを開きます。 このファイルは、**app > manifests** にあります。 次のように、インターネット・アクセスと場所アクセスの許可を `<manifest>` 要素の下に追加します。
{: android}

  ```xml
   <uses-permission android:name="android.permission.INTERNET" />

  ```
  {: codeblock}
  SDK バージョン 1.2 以降を使用している場合、`AndroidManifest.xml` ファイルの `<application>` 要素内に以下の行を挿入する必要があります。
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

#### ステップ 2: 収集したい分析データのタイプに基づいて Android アプリケーションをインスツルメントする
{: #instrument_app_based_on_data_android }
{: android}

1. 分析データをキャプチャーして Mobile Analytics サービスに送信するための初期化をアプリケーションに行います。  最初に、以下の `import` ステートメントをアプリケーションまたはアクティビティー・クラスの先頭に追加します。
{: android}

   ```Java
    import com.worklight.common.Logger;
    import com.worklight.common.WLAnalytics;
   ```
   {: codeblock}
   {: android}

   次に、Mobile Analytics クライアント SDK を使用して、分析データのキャプチャーをセットアップまたは初期化します。  
   初期化コードを、Android アプリケーションのメイン・アクティビティーの `onCreate` メソッド、または、アプリケーション・プロジェクトにとって最も適切な場所に追加します。
   {: android}

   ```Java
    WLAnalytics.init(this.getApplication());
   ```
   {: codeblock}
   {: android}

   この init メソッドを呼び出す前に、デバイスが MobileFoundation サービスで認証と許可を受けるために必要なコードをアプリケーションに埋め込んでおく必要があります。  このステップは、Analytics データをキャプチャーする場合に限らず、Mobile Foundation サービスを使用するあらゆるアプリケーションに必要になる共通のステップです。 <!--  Refer <need to link doc that talks about auth> -->
   {: android}

   初期化が完了したら、アプリケーションはデバイス情報や Mobile Analytics SDK ログをキャプチャーできます。他のコードを追加する必要はありません。  これ以降のセクションで説明している API とコードはオプションです。キャプチャーしたい分析データの種類に応じて追加するとよいでしょう。
   {: android}

2. アプリケーションのセッションやクラッシュの情報などのアプリケーション・ライフサイクル・イベントをキャプチャーするには、以下のコード行を追加してアプリケーションを構成します。
  ```Java
    WLAnalytics.addDeviceEventListener(WLAnalytics.DeviceEvent.LIFECYCLE);
  ```
  {: codeblock}
  {: android}

3. アプリケーションのネットワーク対話をキャプチャーするネットワーク・イベントをキャプチャーするには、以下のコード行を追加してアプリケーションを構成します。
  ```Java
    WLAnalytics.addDeviceEventListener(WLAnalytics.DeviceEvent.NETWORK);
  ```
  {: codeblock}
  {: android}

4. これで、分析データをキャプチャーするようにアプリケーションが初期化されました。  次は、キャプチャーされたデータを Mobile Analytics サービスに送信する必要があります。
   分析データを Mobile Analytics サービスに送信するには、以下の API を使用します。
   ```java
    WLAnalytics.send();
   ```
   {: codeblock}
   {: android}

  アプリケーション・フローのどこでこの API を呼び出して、キャプチャーされた分析データを Mobile Analytics サービスに送信するかは、自由に決めることができます。  送信されるまでの間、キャプチャーされた分析データはすべてデバイス上にローカルに保管されます。
  {: android}

5. アプリケーション・ログをキャプチャーして Mobile Analytics サービスに送信するには、Mobile Analytics クライアント SDK のロガー API を使用します。     
   Mobile Analytics クライアント SDK のロギング・フレームワークでは、以下のログ・レベルがサポートされます。説明が少ないログ・レベルから多くなる順に、お勧めする使用ガイドラインと一緒にリストしています。
    * FATAL - リカバリー不能なクラッシュまたはハングの場合に使用します。 FATAL レベルは、リカバリー不能エラー (ユーザーにはアプリケーション・クラッシュに見えます) のロギングのために予約されています
    * ERROR - 予期しない例外または予期しないネットワーク・プロトコル・エラーの場合に使用します
    * WARN - 推奨されない API の使用やネットワーク応答の遅延など、致命的なエラーではないと考えられる使用上の警告をログに記録する場合に使用します
    * INFO - 初期化イベントや、重要と思われるが緊急ではないその他のデータを報告する場合に使用します
    * DEBUG - アプリケーションの欠陥を解決しようとしている開発者の役に立つデバッグ・ステートメントを報告するために使用します。
    ロガー・レベルを DEBUG に設定した場合は、Mobile Analytics クライアント SDK のログも報告され、ログの送信時に含められます。
   {: note}
   {: android}

   以下のコード・スニペットは、Android の場合のロガーの使用例を示しています。
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

6.  ユーザーのオンボーディング・パターン (新規ユーザーとリピート・ユーザーの比較) に関する洞察を得るには、ユーザー ID とアプリケーション・セッションを紐付ける必要があります。  このステップは、以下の API を呼び出して行うことができます
    ```java
      WLAnalytics.setUserContext("userName or userIdentity");
    ```
    {: codeblock}
    {: android}

    キャプチャーされてローカルに保管されているすべての分析データは、WLAnalytics.send() API を呼び出さないと Mobile Analytics サービスに送信されません。
    {: android}

7.  呼び出された HTTP API、要求の数、平均応答時間などの、アプリケーションの HTTP ネットワーク対話パターンに関する洞察を得るには、Mobile Foundation サービス・クライアント SDK の WLResourceRequest クラスを使用して HTTP 呼び出しを行う必要があります。  ネットワーク・イベントをキャプチャーするように Analytics を初期化した場合でも、`WLResourceRequest` を使用することによって、Mobile Analytics をネットワーク呼び出しにフックして関連データをキャプチャーすることができます。  以下のコードを実行して HTTP 呼び出しを行います。
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

8.  クラッシュ分析とログを取得する場合は、アプリケーションの開始中に以下の API を呼び出して前のセッションがクラッシュしたかどうかを調べ、その結果に応じて、キャプチャーしたクラッシュ・ログを Mobile Analytics サービスに送信することができます。
    ```java
      if (Logger.isUnCaughtExceptionDetected()) {
            //add any other handling you may want and call the following APIs
            WLAnalytics.send();
            Logger.send();
      }
    ```
    {: codeblock}
    {: android}

9.  カスタム分析を定義し、クライアント SDK に用意されている分析データの他に独自の分析データを定義するには、カスタム・ロギング API を使用します。
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

    ログに記録されたカスタム・データを、Mobile Analytics コンソールで定義できるカスタム・グラフ上にプロットし、カスタムの洞察を引き出すことができます。
    {: android}

10. アプリ内ユーザー・フィードバックを使用して、アプリケーションのパフォーマンス分析を深めることができます。 アプリケーションの**ユーザーおよびテスター**からアプリの所有者にコンテキスト・フィードバックが豊富に提供されるようになります。 **アプリの所有者**が、アプリケーションの使用体験に関するフィードバックをユーザーからリアルタイムで取得し、**アプリ所有者**や**開発者**が、そのフィードバックに対処することができます。  この機能により、非常に俊敏にアプリケーションを刷新できます。  例えばボタンのクリックやメニュー項目の選択を処理する際に、アプリケーションのアクション・ハンドラー内で、以下の API を使用して、アプリケーションを対話式フィードバック・モードに切り替えます。
    ```java
        WLAnalytics.triggerFeedbackMode();
    ```
    {: codeblock}
    {: android}

### iOS アプリケーションのインスツルメント
{: #instrument_ios_app}
{: ios}

iOS アプリケーションをインスツルメントする手順は以下のとおりです。
{: ios}

#### ステップ 1: iOS 用の Mobile Analytics クライアント SDK をインポートしてインストールする
{: #install_analytics_sdk_ios }
{: ios}

[![CocoaPods 互換](https://img.shields.io/cocoapods/v/IBMMobileFirstPlatformFoundation.svg)](https://cocoapods.org/pods/IBMMobileFirstPlatformFoundation)
[![CocoaPods 互換](https://img.shields.io/cocoapods/v/IBMMobileFirstPlatformFoundationAnalytics.svg)](https://cocoapods.org/pods/IBMMobileFirstPlatformFoundationAnalytics)
{: ios}

Swift SDK は iOS と watchOS で使用できます。
{: ios}

1. Xcode プロジェクトのルートにある、既存の podfile に以下を追加します
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

2. コマンド・ライン・ウィンドウから Xcode プロジェクトのルートに移動し、以下のコマンドを実行します
   ```bash
   pod update
   ```
   {: codeblock}
   {: ios}

#### ステップ 2: 収集したい分析データのタイプに基づいて iOS アプリケーションをインスツルメントする
{: #instrument_app_based_on_data_ios }
{: ios}

1. Mobile Analytics サービスにログを送信するための初期化をアプリケーションに行います。
   Swift SDK は iOS と watchOS で使用できます。 以下の `import` ステートメントを `AppDelegate.swift` プロジェクト・ファイルの先頭に追加して、`BMSCore` と `BMSAnalytics` のフレームワークをインポートします。
    ```Swift
    import IBMMobileFirstPlatformFoundation
    ```
    {: codeblock}
    {: ios}

   次に、Mobile Analytics クライアント SDK を使用して、分析データのキャプチャーをセットアップまたは初期化します。
   初期化コードを、アプリケーション・プロジェクトにとって最も適切な場所に追加します。
   {: ios}

   ```Swift
    WLAnalytics.init()
   ```
   {: codeblock}
   {: ios}

   この init メソッドを呼び出す前に、デバイスが MobileFoundation サービスで認証と許可を受けるために必要なコードをアプリケーションに埋め込んでおく必要があります。  このステップは、Analytics データをキャプチャーする場合に限らず、Mobile Foundation サービスを使用するあらゆるアプリケーションに必要になる共通のステップです。 <!--  Refer <need to link doc that talks about auth> -->
   {: ios}

   初期化が完了したら、アプリケーションはデバイス情報や Mobile Analytics SDK ログをキャプチャーできます。他のコードを追加する必要はありません。  これ以降のセクションで説明している API とコードはオプションです。キャプチャーしたい分析データの種類に応じて追加するとよいでしょう。
   {: ios}

2. アプリケーションのセッションやクラッシュの情報などのアプリケーション・ライフサイクル・イベントをキャプチャーするには、以下のコード行を追加してアプリケーションを構成します。
    ```Swift
      WLAnalytics.sharedInstance().addDeviceEventListener(LIFECYCLE)
    ```
    {: codeblock}
    {: ios}

3. アプリケーションのネットワーク対話をキャプチャーするネットワーク・イベントをキャプチャーするには、以下のコード行を追加してアプリケーションを構成します。
    ```Swift
      WLAnalytics.sharedInstance().addDeviceEventListener(NETWORK)
    ```
    {: codeblock}
    {: ios}

4. これで、分析データをキャプチャーするようにアプリケーションが初期化されました。  次は、キャプチャーされたデータを Mobile Analytics サービスに送信する必要があります。
   分析データを Mobile Analytics サービスに送信するには、以下の API を使用します。
   ```Swift
    WLAnalytics.sharedInstance().send();
   ```
   {: codeblock}
   {: ios}

    アプリケーション・フローのどこでこの API を呼び出して、キャプチャーされた分析データを Mobile Analytics サービスに送信するかは、自由に決めることができます。  送信されるまでの間、キャプチャーされた分析データはすべてローカルのデバイス上に保管されます。
    {: ios}

5. アプリケーション・ログをキャプチャーして Mobile Analytics サービスに送信するには、Mobile Analytics クライアント SDK のロガー API を使用します。
   Mobile Analytics クライアント SDK のロギング・フレームワークでは、以下のログ・レベルがサポートされます。説明が少ないログ・レベルから多くなる順に、お勧めする使用ガイドラインと一緒にリストしています。
    * FATAL - リカバリー不能なクラッシュまたはハングの場合に使用します。 FATAL レベルは、リカバリー不能エラー (ユーザーにはアプリケーション・クラッシュに見えます) のロギングのために予約されています
    * ERROR - 予期しない例外または予期しないネットワーク・プロトコル・エラーの場合に使用します
    * WARN - 推奨されない API の使用やネットワーク応答の遅延など、致命的なエラーではないと考えられる使用上の警告をログに記録する場合に使用します
    * INFO - 初期化イベントや、重要と思われるが緊急ではないその他のデータを報告する場合に使用します
    * DEBUG - アプリケーションの欠陥を解決しようとしている開発者の役に立つデバッグ・ステートメントを報告するために使用します。
    ロガー・レベルを DEBUG に設定した場合は、Mobile Analytics クライアント SDK のログも報告され、ログの送信時に含められます。
   {: note}
   {: ios}

    iOS アプリケーションにロギング機能を追加するために、[このチュートリアル ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/application-development/client-side-log-collection/ios/) のロガーのコード・スニペットに従ってください。

6.  ユーザーのオンボーディング・パターン (新規ユーザーとリピート・ユーザーの比較) に関する洞察を得るには、ユーザー ID とアプリケーション・セッションを紐付ける必要があります。  この紐付けは、以下の API を呼び出して行うことができます
    ```Swift
      WLAnalytics.sharedInstance().setUserContext("userName or userIdentity")
    ```
    {: codeblock}
    {: ios}

    キャプチャーされてローカルに保管されているすべての分析データは、`WLAnalytics.sharedInstance().send()` API を呼び出さないと Mobile Analytics サービスに送信されません。
    {: ios}

7.  呼び出された HTTP API、要求の数、平均応答時間などの、アプリケーションの HTTP ネットワーク対話パターンに関する洞察を得るには、Mobile Foundation サービス・クライアント SDK の WLResourceRequest クラスを使用して HTTP 呼び出しを行う必要があります。  ネットワーク・イベントをキャプチャーするように Analytics を初期化した場合でも、`WLResourceRequest` を使用することによって、Mobile Analytics をネットワーク呼び出しにフックして関連データをキャプチャーすることができます。  以下のコードを実行して HTTP 呼び出しを行います。
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

8.  クラッシュ分析とログを取得する場合は、アプリケーションの開始中に以下の API を呼び出して前のセッションがクラッシュしたかどうかを調べ、キャプチャーしたクラッシュ・ログを Mobile Analytics サービスに送信することができます。
    ```Swift
      if (OCLogger.isUnCaughtExceptionDetected()) {
            //add any other handling you may want and call the following APIs
            WLAnalytics.sharedInstance().send();
            OCLogger.send();
      }
    ```
    {: codeblock}
    {: ios}

9.  カスタム分析を定義し、クライアント SDK に用意されている分析データの他に独自の分析データを定義するには、カスタム・ロギング API を使用します。
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

    ログに記録されたカスタム・データを、Mobile Analytics コンソールで定義できるカスタム・グラフ上にプロットし、カスタムの洞察を引き出すことができます。
    {: ios}

10. アプリ内ユーザー・フィードバックを使用して、アプリケーションのパフォーマンス分析を深めることができます。   アプリケーションの**ユーザーおよびテスター**からアプリの所有者にコンテキスト・フィードバックが豊富に提供されるようになります。 **アプリの所有者**が、アプリケーションの使用体験に関するフィードバックをユーザーからリアルタイムで取得し、**アプリ所有者**や**開発者**が、そのフィードバックに対処することができます。  この機能により、非常に俊敏にアプリケーションを刷新できます。  例えばボタンのクリックやメニュー項目の選択を処理する際に、アプリケーションのアクション・ハンドラー内で、以下の API を使用して、アプリケーションを対話式フィードバック・モードに切り替えます。
    ```Swift
        WLAnalytics.sharedInstance().triggerFeedbackMode();
    ```
    {: codeblock}
    {: ios}

### Cordova アプリケーションのインスツルメント
{: #instrument_cordova_app}
{: cordova}
Cordova アプリケーションをインスツルメントする手順は以下のとおりです。
{: cordova}

#### ステップ 1: Cordova 用の Foundation と Analytics のクライアント SDK プラグインをインポートしてインストールする
{: #install_analytics_sdk_cordova }
Mobile Analytics Cordova プラグインを使用して、モバイル・アプリケーションをインスツルメントできます。
{: cordova}

#### Cordova クライアント SDK をインストールする
{: #install_cordova_sdk}
{: cordova}

1. [Cordova ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](http://cordova.apache.org/#getstarted){: new_window} プロジェクトを作成するか、既存のプロジェクトを開きます。
    {: cordova}

2. 選択した Android または iOS プラットフォームを Cordova アプリケーションに追加します。 コマンド・ラインから、以下のコマンドのいずれかまたは両方を実行します。<br/>
    **Android** :
    ```
    cordova platform add android
    ```
    {: codeblock}
    {: cordova}

    **iOS** :
    ```
    cordova platform add ios
    ```
    {: codeblock}
    {: cordova}

3. Foundation クライアント SDK プラグイン `cordova-plugin-mfp` を追加します。
   ```bash
   cordova plugin add cordova-plugin-mfp
   ```
   {: codeblock}
   {: cordova}
4. アプリ内フィードバック機能用のオプションの Analytics クライアント SDK プラグイン `cordova-plugin-mfp-analytics` をアプリに追加します
    {: cordova}
    ```bash
    cordova plugin add cordova-plugin-mfp-analytics
    ```
    {: codeblock}
    {: cordova}
5. 次のコマンドを実行して、プラグインが正常にインストールされたことを確認します。
    {: cordova}
    ```bash
    cordova plugin list
    ```
    {: codeblock}
    {: cordova}

#### ステップ 2: 収集したい分析データのタイプに基づいて Cordova アプリケーションをインスツルメントする
{: #instrument_app_based_on_data_cordova }
{: cordova}

1. Cordova アプリケーションの場合は、初期化が組み込まれているので、セットアップは必要ありません。

   以下の分析メソッドを呼び出す前に、デバイスが MobileFoundation サービスで認証および許可を受けるために必要なコードをアプリケーションに埋め込んでおく必要があります。このステップは、Analytics データをキャプチャーする場合に限らず、Mobile Foundation サービスを使用するあらゆるアプリケーションに必要になる共通のステップです。 <!--  Refer <need to link doc that talks about auth> -->
   {: cordova}

   初期化が完了したら、アプリケーションはデバイス情報や Mobile Analytics SDK ログをキャプチャーできます。他のコードを追加する必要はありません。  これ以降のセクションで説明している API とコードはオプションです。キャプチャーしたい分析データの種類に応じて追加するとよいでしょう。
   {: cordova}

2. ライフサイクル・イベントのキャプチャーを有効にするには、Cordova アプリケーションのネイティブ・プラットフォームで初期化を行う必要があります。
    * Android プラットフォームの場合:
      * **「[Cordova アプリケーションのルート・フォルダー]」→「platforms」→「android」→「src」→「com」→「sample」→「[アプリ名]」→「MainActivity.java」**ファイルを開きます。
      * `onCreate` メソッドを見つけ、以下のステップに従って `LIFECYCLE` と `NETWORK` のアクティビティーを有効にします。
        * クライアント・ライフサイクル・イベントのロギングを有効にするには、以下の行を追加します。
          ```Java
          WLAnalytics.addDeviceEventListener(DeviceEvent.LIFECYCLE);
          ```
          {: codeblock}
          {: cordova}
        * クライアント・ネットワーク・イベントのロギングを有効にするには、以下の行を追加します。
          ```Java
          WLAnalytics.addDeviceEventListener(DeviceEvent.NETWORK);
          ```
          {: codeblock}
          {: cordova}
      * コマンド `cordova build` を実行して、Cordova プロジェクトを作成します。
    * iOS プラットフォームの場合:
      * **「[Cordova アプリケーションのルート・フォルダー]」→「platforms」→「ios」→「Classes」**フォルダーを開き、**AppDelegate.swift** (Swift) ファイルを見つけます。
      * 以下のステップを実行して、`LIFECYCLE` と `NETWORK` のアクティビティーを有効にします。
        * クライアント・ライフサイクル・イベントのロギングを有効にするには、以下の行を追加します。
          ```Swift
          WLAnalytics.sharedInstance().addDeviceEventListener(LIFECYCLE);
          ```
          {: codeblock}
          {: cordova}
        * クライアント・ネットワーク・イベントのロギングを有効にするには、以下の行を追加します。
          ```Swift
          WLAnalytics.sharedInstance().addDeviceEventListener(NETWORK);
          ```
          {: codeblock}
          {: cordova}
      * コマンド `cordova build` を実行して、Cordova プロジェクトを作成します。

3. これで、分析データを収集するようにアプリケーションが初期化されました。 次は、分析データを Mobile Analytics に送信できます。
   使用状況分析の記録と送信を開始するには、以下の API を使用します。
   ```Javascript
   // Send recorded usage analytics to the Mobile Analytics

   WL.Analytics.send();
   ```
   {: codeblock}
   {: cordova}

4. アプリケーション・ログをキャプチャーして Mobile Analytics サービスに送信するには、Mobile Analytics クライアント SDK のロガー API を使用します。
   Mobile Analytics クライアント SDK のロギング・フレームワークでは、以下のログ・レベルがサポートされます。説明が少ないログ・レベルから多くなる順に、お勧めする使用ガイドラインと一緒にリストしています。
    * FATAL - リカバリー不能なクラッシュまたはハングの場合に使用します。 FATAL レベルは、リカバリー不能エラー (ユーザーにはアプリケーション・クラッシュに見えます) のロギングのために予約されています
    * ERROR - 予期しない例外または予期しないネットワーク・プロトコル・エラーの場合に使用します
    * WARN - 推奨されない API の使用やネットワーク応答の遅延など、致命的なエラーではないと考えられる使用上の警告をログに記録する場合に使用します
    * INFO - 初期化イベントや、重要と思われるが緊急ではないその他のデータを報告する場合に使用します
    * DEBUG - アプリケーションの欠陥を解決しようとしている開発者の役に立つデバッグ・ステートメントを報告するために使用します。
    ロガー・レベルを DEBUG に設定した場合は、Mobile Analytics クライアント SDK のログも報告され、ログの送信時に含められます。
    * TRACE - メソッドの入り口点および出口点に使用
   {: note}
   {: cordova}

   以下のコード・スニペットは、Cordova の場合のロガーの使用例を示しています。
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

5.  ユーザーのオンボーディング・パターン (新規ユーザーとリピート・ユーザーの比較) に関する洞察を得るには、ユーザー ID とアプリケーション・セッションを紐付ける必要があります。  JavaScript レベルで使用できる API は特にありません。 しかしこの操作は、以下の API を呼び出して、ネイティブ・コード内で実行できます。

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

    キャプチャーされてローカルに保管されているすべての分析データは、JavaScript レベルの WL.Analytics.send() API、Android ネイティブの WLAnalytics.send() API、または iOS ネイティブの WLAnalytics.sharedInstance().send() API を呼び出さないと Mobile Analytics サービスに送信されません。
    {: cordova}

6. 呼び出された HTTP API、要求の数、平均応答時間などの、アプリケーションの HTTP ネットワーク対話パターンに関する洞察を得るには、Mobile Foundation サービス・クライアント SDK の WLResourceRequest クラスを使用して HTTP 呼び出しを行う必要があります。  ネットワーク・イベントをキャプチャーするように Analytics を初期化した場合でも、`WLResourceRequest` を使用することによって、Mobile Analytics をネットワーク呼び出しにフックして関連データをキャプチャーすることができます。  以下のコードを使用して HTTP 呼び出しを行います。
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

7. カスタム分析を定義し、クライアント SDK に用意されている分析データの他に独自の分析データを定義するには、カスタム・ロギング API を使用します。
    ```Javascript
        //create custom data as key value pair
        WL.Analytics.log({"FromPage" : 'LoginPage'});

        //Send the captured custom data and log to the Mobile Analytics service
        WL.Analytics.send();
    ```
    {: codeblock}
    {: cordova}

    ログに記録されたカスタム・データを、Mobile Analytics コンソールで定義できるカスタム・グラフ上にプロットし、カスタムの洞察を引き出すことができます。
    {: cordova}

8. アプリ内ユーザー・フィードバックを使用して、アプリケーションのパフォーマンス分析を深めることができます。   アプリケーションの**ユーザーおよびテスター**からアプリの所有者にコンテキスト・フィードバックが豊富に提供されるようになります。 **アプリの所有者**が、アプリケーションの使用体験に関するフィードバックをユーザーからリアルタイムで取得し、**アプリ所有者**や**開発者**が、そのフィードバックに対処することができます。  このステップにより、非常に俊敏にアプリケーションを刷新できます。  例えばボタンのクリックやメニュー項目の選択を処理する際に、アプリケーションのアクション・ハンドラー内で、以下の API を使用して、アプリケーションを対話式フィードバック・モードに切り替えます。
    ```Javascript
        WL.Analytics.triggerFeedbackMode();
    ```
    {: codeblock}
    {: cordova}

### Web アプリケーションのインスツルメント
{: #instrument_web_app}
{: web}

Web アプリケーションをインスツルメントする手順は以下のとおりです。
{: web}

#### ステップ 1: Web 用の Mobile Analytics クライアント SDK をインポートしてインストールする
{: #install_analytics_sdk_web }
{: web}

#### Web クライアント SDK をインストールする
{: #install_web_sdk}
Mobile Analytics SDK を使用して、Web アプリケーションをインスツルメントできます。
{: web}

1. Web アプリケーションのルート・フォルダーに移動し、以下のコマンドを実行します。 [npm](https://www.npmjs.com/package/ibm-mfp-web-sdk) を使用して SDK を Web アプリケーションに追加します。
    ```bash
    npm install ibm-mfp-web-sdk
    ```
    {: codeblock}
    {: web}


#### ステップ 2: 収集したい分析データのタイプに基づいて Web アプリケーションをインスツルメントする
{: #instrument_app_based_on_data_web }
{: web}

1. メイン HTML ページの `head` 要素で ibmmfpf.js ファイルと ibmmfpfanalytics.js ファイルを参照します

    ```html
      <head>
        ....
        <script type="text/javascript" src="node_modules/ibm-mfp-web-sdk/ibmmfpf.js"></script>
        <script type="text/javascript" src="node_modules/ibm-mfp-web-sdk/lib/analytics/ibmmfpfanalytics.js"></script>
      </head>
    ```
    {: codeblock}
    {: web}

2. コンテキスト・ルートとアプリケーション ID の値を、Web アプリケーションのメインの JavaScript ファイル内に指定することによって、Mobile Foundation Web SDK を初期化します。

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

   以下の分析メソッドを呼び出す前に、デバイスが MobileFoundation サービスで認証および許可を受けるために必要なコードをアプリケーションに埋め込んでおく必要があります。このステップは、Analytics データをキャプチャーする場合に限らず、Mobile Foundation サービスを使用するあらゆるアプリケーションに必要になる共通のステップです。 <!--  Refer <need to link doc that talks about auth> -->
   {: web}

   初期化が完了したら、アプリケーションはデバイス情報や Mobile Analytics SDK ログをキャプチャーできます。他のコードを追加する必要はありません。  これ以降のセクションで説明している API とコードはオプションです。キャプチャーしたい分析データの種類に応じて追加するとよいでしょう。
   {: web}

3. 以下の行をアプリケーション・ロジックに追加して、アプリケーション・ライフサイクル・イベントとネットワーク・イベントをキャプチャーします。

    ```Javascript
    ibmmfpfanalytics.logger.config({analyticsCapture: true});
    ```
    {: codeblock}
    {: web}

4. これで、分析データをキャプチャーするようにアプリケーションが初期化されました。  次は、キャプチャーされたデータを Mobile Analytics サービスに送信する必要があります。 分析データを Mobile Analytics サービスに送信するには、以下の API を使用します。

   ```Javascript
   ibmmfpfanalytics.send();
   ```
   {: codeblock}
   {: web}

    アプリケーション・フローのどこでこの API を呼び出して、キャプチャーされた分析データを Mobile Analytics サービスに送信するかは、自由に決めることができます。  送信されるまでの間、キャプチャーされた分析データはすべてデバイス上にローカルに保管されます。
    {: web}

5. アプリケーション・ログをキャプチャーして Mobile Analytics サービスに送信するには、Mobile Analytics クライアント SDK のロガー API を使用します。
   Mobile Analytics クライアント SDK のロギング・フレームワークでは、以下のログ・レベルがサポートされます。説明が少ないログ・レベルから多くなる順に、お勧めする使用ガイドラインと一緒にリストしています。
    * FATAL - リカバリー不能なクラッシュまたはハングの場合に使用します。 FATAL レベルは、リカバリー不能エラー (ユーザーにはアプリケーション・クラッシュに見えます) のロギングのために予約されています
    * ERROR - 予期しない例外または予期しないネットワーク・プロトコル・エラーの場合に使用します
    * WARN - 推奨されない API の使用やネットワーク応答の遅延など、致命的なエラーではないと考えられる使用上の警告をログに記録する場合に使用します
    * INFO - 初期化イベントや、重要と思われるが緊急ではないその他のデータを報告する場合に使用します
    * DEBUG - アプリケーションの欠陥を解決しようとしている開発者の役に立つデバッグ・ステートメントを報告するために使用します。
    ロガー・レベルを DEBUG に設定した場合は、Mobile Analytics クライアント SDK のログも報告され、ログの送信時に含められます。
    * TRACE - メソッドの入り口点および出口点に使用
   {: note}
   {: web}

   以下のコード・スニペットは、Web アプリの場合のロガーの使用例を示しています。
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

   Web SDK では、クライアントがレベルを設定することはできません。 サーバー構成プロファイルを取得して構成が変更されるまで、すべてのログがサーバーに送信されます。
   {: note}
   {: web}

6. ユーザーのオンボーディング・パターン (新規ユーザーとリピート・ユーザーの比較) に関する洞察を得るには、ユーザー ID とアプリケーション・セッションを紐付ける必要があります。  これは、以下の API を呼び出して行うことができます
    ```Javascript
    ibmmfpfanalytics.setUserContext("userName or userIdentity");
    ```
    {: codeblock}
    {: web}

    キャプチャーされてローカルに保管されているすべての分析データは、ibmmfpfanalytics.send() API を呼び出さないと Mobile Analytics サービスに送信されません。
    {: web}

7.  呼び出された HTTP API、要求の数、平均応答時間などの、アプリケーションの HTTP ネットワーク対話パターンに関する洞察を得るには、Mobile Foundation サービス・クライアント SDK の WLResourceRequest クラスを使用して HTTP 呼び出しを行う必要があります。  ネットワーク・イベントをキャプチャーするように Analytics を初期化した場合でも、`WLResourceRequest` を使用することによって、Mobile Analytics をネットワーク呼び出しにフックして関連データをキャプチャーすることができます。  以下のコードを使用して HTTP 呼び出しを行います。
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

8.  カスタム分析を定義し、クライアント SDK に用意されている分析データの他に独自の分析データを定義するには、カスタム・ロギング API を使用します。

    ```Javascript
        //custom data is sent with the addEvent method
        ibmmfpfanalytics.addEvent({'FromPage':'LoginPage'});

        //Send the captured custom data and log to the Mobile Analytics service
        ibmmfpfanalytics.send();
    ```
    {: codeblock}
    {: web}

    ログに記録されたカスタム・データを、Mobile Analytics コンソールで定義できるカスタム・グラフ上にプロットし、カスタムの洞察を引き出すことができます。
    {: web}

## 次の作業
{: #next_steps_analytics}

[こちら](https://github.com/MobileFirst-Platform-Developer-Center/mfp-analytics-samples)の単純なサンプルを試してください。  5 分もたたないうちにサンプル・アプリケーションを実行して、この API の実力を感じていただけるでしょう。 さらに Analytics コンソールで、分析がさまざまな洞察としてどのように表示されるかをご覧いただけます。  

Mobile Foundation Analytics Service には、開発者が分析データのインポート (POST) とエクスポート (GET) を行うために使用できる REST API が用意されています。

[こちら](https://ma-server.us-south.mobile-analytics-prod.cloud.ibm.com/analytics-service/)の Swagger Docs で Analytics REST API を実際に使用してみましょう。
