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

# Instrument your app
{: #instrument_your_app}

Mobile Analytics is a feature embedded within the {{ site.data.keyword.mobilefoundation_short }} service. Mobile Analytics provides key application usage and application performance insights for mobile application developers and application owners.

You’ll need to instrument your mobile application to start using the Mobile Analytics feature to monitor your application usage, performance and to get other statistics. Instrumenting you application has the following steps:

1.  Import and install the Mobile Analytics Client SDK.
2.  Instrument your application based on the type of analytics data you want to collect.

The following sections will provide the details for these steps, for each of the supported platforms.

### Instrument an Android application
{: #instrument_android_app}
{: android}
#### Step 1: Import and install the Mobile Analytics Client SDK for Android
{: #install_analytics_sdk_android }
{: android}
[![Maven Central](https://maven-badges.herokuapp.com/maven-central/com.ibm.mobile.foundation/ibmmobilefirstplatformfoundation/badge.svg) ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://maven-badges.herokuapp.com/maven-central/com.ibm.mobile.foundation/ibmmobilefirstplatformfoundation)
[![Maven Central](https://maven-badges.herokuapp.com/maven-central/com.ibm.mobile.foundation/ibmmobilefirstplatformfoundationanalytics/badge.svg) ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://maven-badges.herokuapp.com/maven-central/com.ibm.mobile.foundation/ibmmobilefirstplatformfoundationanalytics)
{: android}

The Mobile Analytics Client SDK is distributed with Gradle, a dependency manager for Android projects. Gradle automatically downloads artifacts from repositories and makes them available to your Android application.
{: android}

1. Create an [Android Studio ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://developer.android.com/sdk/index.html){: new_window} project or open an existing project.
{: android}

2. Open the `build.gradle` file that is in your **app module**.
{: android}

  Your Android project might have two `build.gradle` files: one for the project and one for the app module. Make sure to use the **app module** file.
  {: tip}
  {: android}

3. Find the `Dependencies` section of the `build.gradle` file and add a compile dependency for the {{site.data.keyword.mobileanalytics_short}} Client SDK. Your repositories statement should be similar to the following code example:
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

  The first dependency is for the Mobile Analytics client sdk for capturing and logging application runtime events
  and the second dependency is for enabling in-app user feedback interacting with the application user. The second dependency is only required if you enable in-app user feedback
  {: android}

4. Synchronize your project with Gradle by clicking **Tools &gt; Android &gt; Sync Project with Gradle Files**.
{: android}

5. Open the `AndroidManifest.xml` file for your Android project. You can find this file in **app > manifests**. Add internet access and location access permission under the `<manifest>` element:
{: android}

  ```xml
   <uses-permission android:name="android.permission.INTERNET" />

  ```
  {: codeblock}
  If you're using SDK version greater than >= 1.2, then you need to put the following lines inside the `<application>` element of the `AndroidManifest.xml` file.
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

#### Step 2: Instrument your Android application based on the type of analytics data you want to collect
{: #instrument_app_based_on_data_android }
{: android}

1. Initialize your application to capture and send analytics data to Mobile Analytics service.  Firstly, add the following `import` statements to the beginning of your application or activity class:
{: android}

   ```Java
    import com.worklight.common.Logger;
    import com.worklight.common.WLAnalytics;
   ```
   {: codeblock}
   {: android}

   Next use the Mobile Analytics Client SDK to set up or initialize the capture of analytics data.  
   Add the initialization code in the `onCreate` method of the main activity in your Android application, or in a location that works best for your application project.
   {: android}

   ```Java
    WLAnalytics.init(this.getApplication());
   ```
   {: codeblock}
   {: android}

   Before calling the init method you must ensure that your application embeds the required code to authenticate and authorize the device with the MobileFoundation service.  This is a common step that is required of all Mobile Foundation services applications and is not specific to Analytics data capture. <!--  Refer <need to link doc that talks about auth> -->
   {: android}

   With initialization complete your application is now enabled to capture device information and Mobile Analytics SDK logs with no further code added.  Any further APIs and code discussed in the following sections is optional and can be added based on what sort of analytics data you want to capture.
   {: android}

2. To capture application lifecycle events such as application session and crash information configure your application by adding the following:
  ```Java
    WLAnalytics.addDeviceEventListener(WLAnalytics.DeviceEvent.LIFECYCLE);
  ```
  {: codeblock}
  {: android}

3. To capture network events that capture the applications network interactions configure your application by adding the following:
  ```Java
    WLAnalytics.addDeviceEventListener(WLAnalytics.DeviceEvent.NETWORK);
  ```
  {: codeblock}
  {: android}

4. You have now initialized your application to capture analytics data.  Next, you should send the captured data to the Mobile Analytics service.
   Use the following API to send analytics data to the Mobile Analytics service.
   ```java
    WLAnalytics.send();
   ```
   {: codeblock}
   {: android}

  You are free to decide when to call this API in your application flow to send the captured analytics data to the Mobile Analytics service.  Until this send all captured analytics data is stored locally on the device.
  {: android}

5. To capture and send application logs to the Mobile Analytics service use the Mobile Analytics Client SDK logger APIs.     
   The Mobile Analytics Client SDK logging framework supports the following log levels, which are listed from least to most verbose, with recommended use guidelines:
    * FATAL - Use for unrecoverable crashes or hangs. The FATAL level is reserved for logging unrecoverable errors, which appear to users as an application crash
    * ERROR - Use for unexpected exceptions or unexpected network protocol errors
    * WARN - Use to log usage warnings that aren’t considered critical errors, such as usage of deprecated APIs or slow network response
    * INFO - Use for reporting initialization events and other data that might be important, but not urgent
    * DEBUG - Use for reporting debug statements to help developers resolve application defects.
    When the logger level is set DEBUG, you also get Mobile Analytics Client SDK logs, which are included when you send logs.
   {: note}
   {: android}

   The following code snippets show sample Logger usage for Android:
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

6.  To get insights on user onboarding patterns (new users versus returning users) you must associate a user identity with your application session.  This can be done by calling the following API
    ```java
      WLAnalytics.setUserContext("userName or userIdentity");
    ```
    {: codeblock}
    {: android}

    Note that all analytics data captured and stored locally is sent to the Mobile Analytics service only when the WLAnalytics.send() API is called.
    {: android}

7.  To get insights into your applications HTTP network interaction patterns such as HTTP APIs called, number of requests and average response times you must use the Mobile Foundation Service Client SDK's WLResourceRequest class to make the HTTP calls.  Although you might have initialized Analytics for the capture of Network events, using WLResourceRequest enables the Mobile Analytics to hook into the network calls and capture the relevant data.  Here is how you should make your HTTP calls.
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

8.  To get Crash Analytics and logs you could call the following the API during start of your application to check if the previous session had crashed and accordingly send the capture crash logs to the Mobile Analytics service.
    ```java
      if (Logger.isUnCaughtExceptionDetected()) {
            //add any other handling you may want and call the following APIs
            WLAnalytics.send();
            Logger.send();
      }
    ```
    {: codeblock}
    {: android}

9.  To define Custom Analytics and define your own analytics data over and above what is supported inherently in the Client SDK you could use the custom logging API
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

    The logged custom data can be plotted over custom charts you can define in the Mobile Analytics console to derive custom insights.
    {: android}

10. Use In-App User Feedback to deepen your application performance analysis.   You can enable your application **users and testers** to provide rich contextual feedback to the app owners. **App owners** get real-time feedback from its users on the application usage experience which **App owners** and **Developers** can further act on.  This brings in significant agility to application up-keep.  Use the following API to switch your application to Interactive Feedback Mode in any action handler in your application for example when handling the click of a button or the selection of a menu item.
    ```java
        WLAnalytics.triggerFeedbackMode();
    ```
    {: codeblock}
    {: android}

### Instrument an iOS application
{: #instrument_ios_app}
{: ios}

Steps to instrument your iOS application:
{: ios}

#### Step 1: Import and install the Mobile Analytics Client SDK for iOS
{: #install_analytics_sdk_ios }
{: ios}

[![CocoaPods Compatible](https://img.shields.io/cocoapods/v/IBMMobileFirstPlatformFoundation.svg)](https://cocoapods.org/pods/IBMMobileFirstPlatformFoundation)
[![CocoaPods Compatible](https://img.shields.io/cocoapods/v/IBMMobileFirstPlatformFoundationAnalytics.svg)](https://cocoapods.org/pods/IBMMobileFirstPlatformFoundationAnalytics)
{: ios}

The Swift SDK is available for iOS and watchOS.
{: ios}

1. Add the following to the existing podfile, located at the root of the Xcode project
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

2. From a Command-line window, navigate to the root of the Xcode project and run the following command
   ```bash
   pod update
   ```
   {: codeblock}
   {: ios}

#### Step 2: Instrument your iOS application based on the type of analytics data you want to collect
{: #instrument_app_based_on_data_ios }
{: ios}

1. Initialize your application to enable sending logs to the Mobile Analytics.
   The Swift SDK is available for iOS and watchOS. Import the `BMSCore` and `BMSAnalytics` frameworks by adding the following `import` statements to the beginning of your `AppDelegate.swift` project file:
    ```Swift
    import IBMMobileFirstPlatformFoundation
    ```
    {: codeblock}
    {: ios}

   Next use the Mobile Analytics Client SDK to set up or initialize the capture of analytics data.
   Add the initialization code in a location that works best for your application project.
   {: ios}

   ```Swift
    WLAnalytics.init()
   ```
   {: codeblock}
   {: ios}

   Before calling the init method you must ensure that your application embeds the required code to authenticate and authorize the device with the MobileFoundation service.  This is a common step that is required of all Mobile Foundation services applications and is not specific to Analytics data capture. <!--  Refer <need to link doc that talks about auth> -->
   {: ios}

   With initialization complete your application is now enabled to capture device information and Mobile Analytics SDK logs with no further code added.  Any further APIs and code discussed in the following sections is optional and can be added based on what sort of analytics data you want to capture.
   {: ios}

2. To capture application lifecycle events such as application session and crash information configure your application by adding the following:
    ```Swift
      WLAnalytics.sharedInstance().addDeviceEventListener(LIFECYCLE)
    ```
    {: codeblock}
    {: ios}

3. To capture network events that capture the applications network interactions configure your application by adding the following:
    ```Swift
      WLAnalytics.sharedInstance().addDeviceEventListener(NETWORK)
    ```
    {: codeblock}
    {: ios}

4. You have now initialized your application to capture analytics data.  Next, you should send the captured data to the Mobile Analytics service.
   Use the following API to send analytics data to the Mobile Analytics service.
   ```Swift
    WLAnalytics.sharedInstance().send();
   ```
   {: codeblock}
   {: ios}

    You are free to decide when to call this API in your application flow to send the captured analytics data to the Mobile Analytics service.  Until this send all captured analytics data is stored locally on the device.
    {: ios}

5. To capture and send application logs to the Mobile Analytics service use the Mobile Analytics Client SDK logger APIs.
   The Mobile Analytics Client SDK logging framework supports the following log levels, which are listed from least to most verbose, with recommended use guidelines:
    * FATAL - Use for unrecoverable crashes or hangs. The FATAL level is reserved for logging unrecoverable errors, which appear to users as an application crash
    * ERROR - Use for unexpected exceptions or unexpected network protocol errors
    * WARN - Use to log usage warnings that aren’t considered critical errors, such as usage of deprecated APIs or slow network response
    * INFO - Use for reporting initialization events and other data that might be important, but not urgent
    * DEBUG - Use for reporting debug statements to help developers resolve application defects.
    When the logger level is set DEBUG, you also get Mobile Analytics Client SDK logs, which are included when you send logs.
   {: note}
   {: ios}

    Follow [the tutorial ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://mobilefirstplatform.ibmcloud.com/tutorials/it/foundation/8.0/application-development/client-side-log-collection/ios/) for code snipets for Logger in order to add logging capabilities in iOS applications.

6.  To get insights on user onboarding patterns (new users versus returning users) you must associate a user identity with your application session.  This can be done by calling the following API
    ```Swift
      WLAnalytics.sharedInstance().setUserContext("userName or userIdentity")
    ```
    {: codeblock}
    {: ios}

    Note that all analytics data captured and stored locally is sent to the Mobile Analytics service only when the WLAnalytics.sharedInstance().send() API is called.
    {: ios}

7.  To get insights into your applications HTTP network interaction patterns such as HTTP APIs called, number of requests and average response times you must use the Mobile Foundation Service Client SDK's WLResourceRequest class to make the HTTP calls.  Although you might have initialized Analytics for the capture of Network events, using WLResourceRequest enables the Mobile Analytics to hook into the network calls and capture the relevant data.  Here is how you should make your HTTP calls.
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

8.  To get Crash Analytics and logs you could call the following the API during start of your application to check if the previous session had crashed and accordingly send the capture crash logs to the Mobile Analytics service.
    ```Swift
      if (OCLogger.isUnCaughtExceptionDetected()) {
            //add any other handling you may want and call the following APIs
            WLAnalytics.sharedInstance().send();
            OCLogger.send();
      }
    ```
    {: codeblock}
    {: ios}

9.  To define Custom Analytics and define your own analytics data over and above what is supported inherently in the Client SDK you could use the custom logging API
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

    The logged custom data can be plotted over custom charts you can define in the Mobile Analytics console to derive custom insights.
    {: ios}

10. Use In-App User Feedback to deepen your application performance analysis.   You can enable your application **users and testers** to provide rich contextual feedback to the app owners. **App owners** get real-time feedback from its users on the application usage experience which **App owners** and **Developers** can further act on.  This brings in significant agility to application up-keep.  Use the following API to switch your application to Interactive Feedback Mode in any action handler in your application for example when handling the click of a button or the selection of a menu item.
    ```Swift
        WLAnalytics.sharedInstance().triggerFeedbackMode();
    ```
    {: codeblock}
    {: ios}

### Instrument a Cordova application
{: #instrument_cordova_app}
{: cordova}
Steps to instrument your Cordova application:
{: cordova}

#### Step 1: Import and install the Foundation and Analytics Client SDK plugin for Cordova
{: #install_analytics_sdk_cordova }
The Mobile Analytics Cordova plug-in enables you to instrument your mobile application.
{: cordova}

#### Installing the Cordova Client SDK
{: #install_cordova_sdk}
{: cordova}

1. Create a [Cordova ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://cordova.apache.org/#getstarted){: new_window} project or open an existing project.
    {: cordova}

2. Add Android or iOS platforms of your choice to your Cordova application. Run one or both of the following commands from the command line.<br/>
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

3. Add the Foundation client sdk plugin `cordova-plugin-mfp`.
   ```bash
   cordova plugin add cordova-plugin-mfp
   ```
   {: codeblock}
   {: cordova}
4. Add optional Analytics client sdk plugin `cordova-plugin-mfp-analytics` which is for in-app feedback capability to your app
    {: cordova}
    ```bash
    cordova plugin add cordova-plugin-mfp-analytics
    ```
    {: codeblock}
    {: cordova}
5. Verify that the plug-in installed successfully by running the following command:
    {: cordova}
    ```bash
    cordova plugin list
    ```
    {: codeblock}
    {: cordova}

#### Step 2: Instrument your Cordova application based on the type of analytics data you want to collect
{: #instrument_app_based_on_data_cordova }
{: cordova}

1. In Cordova applications, no setup is required and initialization is built-in.

   Before calling any of the below analytic methods you must ensure that your application embeds the required code to authenticate and authorize the device with the MobileFoundation service.  This is a common step that is required of all Mobile Foundation services applications and is not specific to Analytics data capture. <!--  Refer <need to link doc that talks about auth> -->
   {: cordova}

   With initialization complete your application is now enabled to capture device information and Mobile Analytics SDK logs with no further code added.  Any further APIs and code discussed in the following sections is optional and can be added based on what sort of analytics data you want to capture.
   {: cordova}

2. To enable the capture of the lifecycle events, it must be initialized in the native platform of the Cordova app.
    * For the Android platform:
      * Open the  **[Cordova application root folder] → platforms → android → src → com → sample → [app-name] → MainActivity.java** file.
      * Look for the `onCreate` method and follow the steps below to enable `LIFECYCLE` and `NETWORK` activities:
        * To enable client lifecycle event logging:
          ```Java
          WLAnalytics.addDeviceEventListener(DeviceEvent.LIFECYCLE);
          ```
          {: codeblock}
          {: cordova}
        * To enable client network-event logging:
          ```Java
          WLAnalytics.addDeviceEventListener(DeviceEvent.NETWORK);
          ```
          {: codeblock}
          {: cordova}
      * Build the Cordova project by running the command: `cordova build`.
    * For the iOS platform:
      * Open the **[Cordova application root folder] → platforms → ios → Classes** folder and find the  **AppDelegate.swift** (Swift) file.
      * Follow the steps below to to enable `LIFECYCLE` and `NETWORK` activities.
        * To enable client lifecycle event logging:
          ```Swift
          WLAnalytics.sharedInstance().addDeviceEventListener(LIFECYCLE);
          ```
          {: codeblock}
          {: cordova}
        * To enable client network-event logging:
          ```Swift
          WLAnalytics.sharedInstance().addDeviceEventListener(NETWORK);
          ```
          {: codeblock}
          {: cordova}
      * Build the Cordova project by running the command: `cordova build`.

3. You've now initialized your application to collect analytics data. Next, you can send analytics data to Mobile Analytics.
   Use the following APIs to start recording and sending usage analytics:
   ```Javascript
   // Send recorded usage analytics to the Mobile Analytics

   WL.Analytics.send();
   ```
   {: codeblock}
   {: cordova}

4. To capture and send application logs to the Mobile Analytics service use the Mobile Analytics Client SDK logger APIs.
   The Mobile Analytics Client SDK logging framework supports the following log levels, which are listed from least to most verbose, with recommended use guidelines:
    * FATAL - Use for unrecoverable crashes or hangs. The FATAL level is reserved for logging unrecoverable errors, which appear to users as an application crash
    * ERROR - Use for unexpected exceptions or unexpected network protocol errors
    * WARN - Use to log usage warnings that aren’t considered critical errors, such as usage of deprecated APIs or slow network response
    * INFO - Use for reporting initialization events and other data that might be important, but not urgent
    * DEBUG - Use for reporting debug statements to help developers resolve application defects.
    When the logger level is set DEBUG, you also get Mobile Analytics Client SDK logs, which are included when you send logs.
    * TRACE - used for method entry and exit points
   {: note}
   {: cordova}

   The following code snippets show sample Logger usage for Cordova:
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

5.  To get insights on user onboarding patterns (new users versus returning users) you must associate a user identity with your application session.  There are no specific API available from Javascript level. But this can be done at the native code by calling the following API:

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

    Note that all analytics data captured and stored locally is sent to the Mobile Analytics service only when the WL.Analytics.send() API in javascript level [or] WLAnalytics.send() API in android native [or] WLAnalytics.sharedInstance().send() API in iOS native is called.
    {: cordova}

6. To get insights into your applications HTTP network interaction patterns such as HTTP APIs called, number of requests and average response times you must use the Mobile Foundation Service Client SDK's WLResourceRequest class to make the HTTP calls.  Although you might have initialized Analytics for the capture of Network events, using WLResourceRequest enables the Mobile Analytics to hook into the network calls and capture the relevant data.  Here is how you should make your HTTP calls.
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

7. To define Custom Analytics and define your own analytics data over and above what is supported inherently in the Client SDK you could use the custom logging API
    ```Javascript
        //create custom data as key value pair
        WL.Analytics.log({"FromPage" : 'LoginPage'});

        //Send the captured custom data and log to the Mobile Analytics service
        WL.Analytics.send();
    ```
    {: codeblock}
    {: cordova}

    The logged custom data can be plotted over custom charts you can define in the Mobile Analytics console to derive custom insights.
    {: cordova}

8. Use In-App User Feedback to deepen your application performance analysis.   You can enable your application **users and testers** to provide rich contextual feedback to the app owners. **App owners** get real-time feedback from its users on the application usage experience which **App owners** and **Developers** can further act on.  This brings in significant agility to application up-keep.  Use the following API to switch your application to Interactive Feedback Mode in any action handler in your application for example when handling the click of a button or the selection of a menu item.
    ```Javascript
        WL.Analytics.triggerFeedbackMode();
    ```
    {: codeblock}
    {: cordova}

### Instrument a Web application
{: #instrument_web_app}
{: web}

Steps to instrument your Web application:
{: web}

#### Step 1: Import and install the Mobile Analytics Client SDK for Web
{: #install_analytics_sdk_web }
{: web}

#### Installing the Web Client SDK
{: #install_web_sdk}
The Mobile Analytics SDK enables you to instrument your web application.
{: web}

1. Navigate to the root folder of your web application and run the following command. Add the SDK to your web application using [npm](https://www.npmjs.com/package/ibm-mfp-web-sdk)
    ```bash
    npm install ibm-mfp-web-sdk
    ```
    {: codeblock}
    {: web}


#### Step 2: Instrument your Web application based on the type of analytics data you want to collect
{: #instrument_app_based_on_data_web }
{: web}

1. Reference the ibmmfpf.js and ibmmfpfanalytics.js file in the `head` element of your main html page

    ```html
      <head>
        ....
        <script type="text/javascript" src="node_modules/ibm-mfp-web-sdk/ibmmfpf.js"></script>
        <script type="text/javascript" src="node_modules/ibm-mfp-web-sdk/lib/analytics/ibmmfpfanalytics.js"></script>
      </head>
    ```
    {: codeblock}
    {: web}

2. Initialize the Mobile Foundation Web SDK by specifying the context root and application ID values in the main JavaScript file of your web application

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

   Before calling any of the below analytic method you must ensure that your application embeds the required code to authenticate and authorize the device with the MobileFoundation service.  This is a common step that is required of all Mobile Foundation services applications and is not specific to Analytics data capture. <!--  Refer <need to link doc that talks about auth> -->
   {: web}

   With initialization complete your application is now enabled to capture device information and Mobile Analytics SDK logs with no further code added.  Any further APIs and code discussed in the following sections is optional and can be added based on what sort of analytics data you want to capture.
   {: web}

3. Add the following line to your application logic to capture application lifecycle events and network events.

    ```Javascript
    ibmmfpfanalytics.logger.config({analyticsCapture: true});
    ```
    {: codeblock}
    {: web}

4. You have now initialized your application to capture analytics data.  Next, you should send the captured data to the Mobile Analytics service.Use the following API to send analytics data to the Mobile Analytics service.

   ```Javascript
   ibmmfpfanalytics.send();
   ```
   {: codeblock}
   {: web}

    You are free to decide when to call this API in your application flow to send the captured analytics data to the Mobile Analytics service.  Until this send all captured analytics data is stored locally on the device.
    {: web}

5. To capture and send application logs to the Mobile Analytics service use the Mobile Analytics Client SDK logger APIs.
   The Mobile Analytics Client SDK logging framework supports the following log levels, which are listed from least to most verbose, with recommended use guidelines:
    * FATAL - Use for unrecoverable crashes or hangs. The FATAL level is reserved for logging unrecoverable errors, which appear to users as an application crash
    * ERROR - Use for unexpected exceptions or unexpected network protocol errors
    * WARN - Use to log usage warnings that aren’t considered critical errors, such as usage of deprecated APIs or slow network response
    * INFO - Use for reporting initialization events and other data that might be important, but not urgent
    * DEBUG - Use for reporting debug statements to help developers resolve application defects.
    When the logger level is set DEBUG, you also get Mobile Analytics Client SDK logs, which are included when you send logs.
    * TRACE - used for method entry and exit points
   {: note}
   {: web}

   The following code snippets show sample Logger usage for Web app:
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

   For the Web SDK the level cannot be set by the client. All logging is sent to the server until the configuration is changed by retrieving the server configuration profile.
   {: note}
   {: web}

6. To get insights on user onboarding patterns (new users versus returning users) you must associate a user identity with your application session.  This can be done by calling the following API
    ```Javascript
    ibmmfpfanalytics.setUserContext("userName or userIdentity");
    ```
    {: codeblock}
    {: web}

    Note that all analytics data captured and stored locally is sent to the Mobile Analytics service only when the ibmmfpfanalytics.send() API is called.
    {: web}

7.  To get insights into your applications HTTP network interaction patterns such as HTTP APIs called, number of requests and average response times you must use the Mobile Foundation Service Client SDK's WLResourceRequest class to make the HTTP calls.  Although you might have initialized Analytics for the capture of Network events, using WLResourceRequest enables the Mobile Analytics to hook into the network calls and capture the relevant data.  Here is how you should make your HTTP calls.
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

8.  To define Custom Analytics and define your own analytics data over and above what is supported inherently in the Client SDK you could use the custom logging API:

    ```Javascript
        //custom data is sent with the addEvent method
        ibmmfpfanalytics.addEvent({'FromPage':'LoginPage'});

        //Send the captured custom data and log to the Mobile Analytics service
        ibmmfpfanalytics.send();
    ```
    {: codeblock}
    {: web}

    The logged custom data can be plotted over custom charts you can define in the Mobile Analytics console to derive custom insights.
    {: web}

## What to do next?
{: #next_steps_analytics}

Try a simple sample from [here](https://github.com/MobileFirst-Platform-Developer-Center/mfp-analytics-samples).  In less than 5 minutes you will be able to run the sample application and get a feel of what the API discussed on this page do.  You will also be able to notice how analytics show up as different insights on the Analytics console.  

Mobile Foundation Analytics Service provides REST APIs to help developers with importing (POST) and exporting (GET) analytics data.

Try out the analytics REST API on Swagger Docs from [here](https://mobile-analytics-dashboard.ng.bluemix.net/analytics-service/).
