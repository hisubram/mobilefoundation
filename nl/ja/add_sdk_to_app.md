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

#	アプリへの Mobile Foundation SDK の追加
{: #add_sdk_to_app}

### アプリへの Android SDK の追加
{: android}

Android Studio を開き、Android ビューを選択し、**「Gradle Scripts」**を選択し、`build.gradle (Module: app)` ファイルを選択してから、以下の手順に従って Android SDK を android アプリケーションに追加します。
{: android}

1. 次の行を `dependencies` セクションに追加します。
   ```bash
   compile 'com.ibm.mobile.foundation:ibmmobilefirstplatformfoundation:8.0.+'
   ```
   {: codeblock}
   {: android}
2. 次の行を `android` セクションに追加します。
   ```
   packagingOptions {
              pickFirst 'META-INF/ASL2.0'
              pickFirst 'META-INF/LICENSE'
        pickFirst 'META-INF/NOTICE'
   }
  ```
  {: codeblock}
  {: android}
3. Android ビューで、**app → manifests → AndroidManifest.xml** ファイルを開きます。 次のアクセス権を `application` エレメントの上に追加します。
   ```xml
   <uses-permission android:name="android.permission.INTERNET"/>
   <uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>
   ```
   {: codeblock}
   {: android}
4. 既存の `activity` エレメントの隣に MobileFirst UI アクティビティーを追加します。
   ```xml
   <activity android:name="com.worklight.wlclient.ui.UIActivity" />
   ```
   {: codeblock}
   {: android}


### アプリへの iOS SDK の追加
{: ios}

これが IBM Mobile Foundation のコアを成す SDK です。Mobile Foundation のセキュリティー、許可、ロギング、アダプター呼び出しなどの主要な機能を実装する API 群で構成されています。 iOS アプリケーションに iOS SDK を追加するには、以下の手順に従います。
{: ios}

1. iOS アプリのルート・フォルダーに移動し、次のコマンドを実行して Podfile を作成します。
    ```bash
    pod init
    ```
    {: codeblock}
    {: ios}
2. 好みのコード・エディターを使用して、この Podfile を開きます。
   ```bash
   use_frameworks!
   platform :ios, 8.0
   target "ProjectName" do
       pod 'IBMMobileFirstPlatformFoundation'
   end
   ```
   {: codeblock}
   {: ios}
3. 次のコマンドを使用してポッドを更新します。
   ```bash
   pod update
   ```
   {: codeblock}
   {: ios}
4. 次のコマンドを使用してポッドをインストールします。
   ```bash
   pod install
   ```
   {: codeblock}
   {: ios}
5. [ProjectName].xcworkspace を開いて、プロジェクトを Xcode で開きます。
6. `mfpclient.plist` ファイルを Xcode ワークスペースに追加します。
7. コマンド・プロンプトで、プロジェクト・フォルダーに移動し、アプリを Mobile Foundation インスタンスに登録します。
   ```bash
   mfpdev app register
   ```
   {: codeblock}
   {: ios}

### アプリケーションへの Cordova SDK の追加
{: cordova}

これが IBM Mobile Foundation のコアを成す SDK です。Mobile Foundation のセキュリティー、許可、ロギング、アダプター呼び出しなどの主要な機能を実装する API 群で構成されています。 アプリケーションに Cordova SDK を追加するには、以下の手順に従います。
{: cordova}

1. Cordova プロジェクトを作成します。
   ```bash
   cordova create Hello com.example.helloworld HelloWorld
   ```
   {: codeblock}
   {: cordova}
2. ディレクトリーを Cordova プロジェクトのルートに変更します。
   ```bash
   cd Hello
   ```
   {: codeblock}
   {: cordova}
3. Cordova CLIを使用して、サポートされているプラットフォームを 1 つ以上 Cordova プロジェクトに追加します。
   ```bash
   cordova platform add ios|android|windows
   ```
   {: codeblock}
   {: cordova}
4. Mobile Foundation のコア Cordova プラグインを追加します。
   ```bash
   cordova plugin add cordova-plugin-mfp
   ```
   {: codeblock}
   {: cordova}
5. 次のコマンドを実行して、アプリケーションのリソースを準備します。
   ```bash
   cordova prepare
   ```
   {: codeblock}
   {: cordova}
6. アプリケーションを Mobile Foundation サーバーに登録します。
   ```bash
   mfpdev app register
   ```
   {: codeblock}
   {: cordova}

### アプリへの React Native SDK プラグインの追加
{: reactnative}

Mobile Foundation の機能を既存の React Native アプリに追加するには、`react-native-ibm-mobilefirst` プラグインをアプリに追加する必要があります。 `react-native-ibm-mobilefirst` プラグインには、Mobile Foundation SDK が含まれています。 React Native アプリケーションに React Native プラグインを追加するには、以下の手順に従います。
{: reactnative}

1. このプラグインを、他の `npm` プラグインを追加するときと同じ方法でアプリに追加します。
   ```bash
   npm install react-native-ibm-mobilefirst --save
   ```
   {: codeblock}
   {: reactnative}
2. 最初の手順は、React Native プロジェクト (*MobileFirstApp* など) を作成する手順です。 React Native CLI を使用して、新規プロジェクトを作成します。
   ```bash
   react-native init MobileFirstApp
   ```
   {: codeblock}
   {: reactnative}
3. 次に、React Native プラグインをアプリに追加します。
   ```bash
   cd MobileFirstApp
   npm install react-native-ibm-mobilefirst --save
   ```
   {: codeblock}
   {: reactnative}
4. ネイティブのすべての依存関係が React Native プロジェクトに追加されるように、プロジェクトをリンクします。
   ```bash
   react-native link
   ```
   {: codeblock}
   {: reactnative}
5. Android の場合は、以下の変更を `AndroidManifest.xml` (`<PROJECT_ROOT>/android/app/src/main/`) に加えます。
   ```xml
   <manifest 
          xmlns:android="http://schemas.android.com/apk/res/android" 
          xmlns:tools="http://schemas.android.com/tools"
          package="com.mobilefirstapp">
   ```
   {: codeblock}
   {: reactnative}
6. **tools:replace='android:allowBackup'** を `application` タグに追加します。
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
7. iOS の場合は、XCode をプロジェクト・ナビゲーターで開き、`mfpclient.plist` を `ios` フォルダーからドラッグ・アンド・ドロップします。 この手順は iOS プラットフォームにのみ適用されます。
{: reactnative}

